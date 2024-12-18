# ReadMe
Paar exemplaren van code binnen privé repositories van Hogeschool Leiden.

# Atelier van der Lelie
Samen met een groep van studenten moest ik als een team een applicatie maken voor het bedrijf Atelier van der Lelie. De algemene opdracht was een applicatie te maken dat kon helpen met de onderhoud en beheer van een oude netwerk van deze organisatie. In deze organisatie wordt textiel en maatwerk producten maken van ondermaten, echter levert dit restanten op die aan opdrachtgevers van deze organisatie wordt geleverd. Meestal wilden een kleine percentage van deze opdrachtgevers de restanten terug krijgen. Het organiseren van de restanten en bepalen of de opdrachtgevers deze restanten terug willen krijgen, het bedrijf vroeg voor een applicatie die daarmee kon helpen aan de team.

Dit moest volgens de onderstaande figma exemplaar die was gemaakt, voordat dit project werd ontwikkeld in Angular.
![Figma Concept Atelier](https://i.postimg.cc/Nj6GXKCX/Afbeelding1.png)

Hier is het daadwerkelijke project tijdens de deadline van de opdrachtgever.
![Ipsen3 Project](https://i.postimg.cc/TYfFYQ5n/Schermafbeelding-2023-12-23-222211.png)

## Toevoegingsforum voor de Gordijnrollenbeheersysteem in Angular
De use-case voor deze functionaliteit is, dat er een methode nodig was om een nieuwe artikel van ondermaten toe te voegen aan een database die door andere leden van de team is opgezet. Deze database was gebaseerd op alleen informatie dat was gegeven in een excel bestand, de waardes die aanwezig waren niet al te concreet. Hierdoor moesten ik en de team waarmee ik werkte op basis van de entiteiten de gegeven van de artikel dat wordt doorgegeven opsplitsen. Dit werd gedaan in twee delen samen met één klant die gebonden wordt aan de bestelling, de indelingen van objecten zijn:
```
  -Artikel (Bindt ArtikelData en ArtikelDescriptie via ids),
  -ArtikelData (Bevat meerdere numerieke waardes),
  -ArtikelDescriptie (Bevat beschrijvingen en extra info),
  -Klant (Klant dat gebonden zal worden aan één Artikel)
```
Achter de knop van "Toevoegen" zit mijn gedeelte van code verborgen in de applicatie. Want de forum moest voorkomen in de vorm van een pop-up in overleg met de team waar ik deel van was.
![Popup](https://i.postimg.cc/50LD7RTk/Schermafbeelding-2023-12-17-210348.png)

![Popup geactiveerd](https://i.postimg.cc/FzH0KcFN/Schermafbeelding-2023-12-23-224732.png)
```HTML
<h2 mat-dialog-title>Afval Toevoegen</h2>
<form (ngSubmit)="onSubmit()" [formGroup]="inputData">
  <mat-dialog-content>
    <mat-grid-list cols="3" rowHeight="75px">
      <!-- Eerste helft van de Object Artikel in het systeem genaamd ArticleData -->
      <div formGroupName="articleData">
        <!-- Elk attribuut gebonden aan de entiteit ArticleData met Validators -->
      </div>
      <!-- Tweede helft van het Object Artikel genaamd ArticleDescription -->
      <div formGroupName="articleDescription">
        <!-- Elk attribuut gebonden aan de entiteit ArticleDescription met Validators -->
      </div>
      <!-- Hier komen de klanten vanuit de database dat gebonden moet worden aan de aangegeven artikel -->
      <mat-grid-tile>
        <!-- Een selector om één klant te kiezen -->
      </mat-grid-tile>

      <mat-grid-tile colspan="3">
        <span *ngIf="inputData.valid && inputData.touched">Deze artikel voldoet nu aan alle eisen!</span>
      </mat-grid-tile>
    </mat-grid-list>
  </mat-dialog-content>
  <mat-dialog-actions align="end">
    <button mat-raised-button color="warn" mat-dialog-close="false" (click)="onCancel()">Close</button>
    <button mat-raised-button color="primary" mat-dialog-close="true" type="submit" [disabled]="!inputData.valid">Confirm</button>
  </mat-dialog-actions>
</form>
```
Een voorbeeld van een Formcontrol binnen de FormGroup van binnen de gehele toevoegingsforum. Er zijn Validators aan toegevoegd om te voorkomen dat er van alles wordt ingevuld dat niet overeenkomt met wat de entiteiten accepteren. Ook zijn er error velden geplaatst om de gebruiker te informeren wat niet kan.
![Errorfields](https://i.postimg.cc/KjPknhxG/Schermafbeelding-2023-12-22-183235.png)
``` HTML
<mat-grid-tile colspan="1">
  <mat-form-field>
    <mat-label>Compositie percentage</mat-label>
    <input matInput type="number" name="composition_percentage" id="composition_percentage" formControlName="composition_percentage">
    <mat-error *ngIf="!inputData.get('articleData.composition.composition_percentage').valid && inputData.get('articleData.composition.composition_percentage').value > 100 && inputData.get('articleData.composition.composition_percentage').value != null">Percentage mag niet boven <b>100</b> zitten</mat-error>
    <mat-error *ngIf="!inputData.get('articleData.composition.composition_percentage').valid && inputData.get('articleData.composition.composition_percentage').value <= 0 && inputData.get('articleData.composition.composition_percentage').value != null">Percentage mag niet onder <b>1</b> zitten</mat-error>
    <mat-error *ngIf="!inputData.get('articleData.composition.composition_percentage').valid && inputData.get('articleData.composition.composition_percentage').touched">Compositie is <b>verplicht</b></mat-error>
    <span matTextSuffix>%</span>
  </mat-form-field>
</mat-grid-tile>
```
Pas als alles valid is, dan mag deze informatie verstuurd worden naar de Back-end en kan de gebruiker de "Confirm" knop klikken.
![Valid Forum](https://i.postimg.cc/Y21XC4YF/Schermafbeelding-2023-12-22-184325.png)

Eerst moesten alle klanten worden opgehaald worden waarvan al een bestelling is uitgevoerd. Echter moest dit gemockt worden, doordat er geen andere applicatie van het bedrijf beschikbaar was om dit voor de team te regelen.
```Typescript
// Haalt alle klanten op in de database die bestellingen heeft.
  ngOnInit() {
    this.getCustomers()
  }

// Benaming doorgeven van een gedeelte URL voor de GET request van de klanten.
  getCustomers() {
    return this.http.getData<Customer>("/api/v2/customer").subscribe(
      (_customerList) => {
        this.customers = _customerList
      }
    );
  }
```
Een benodigde forum met invulvelden voor elk stuk informatie dat een artikel nodig heeft on toegevoegd te worden aan de database en aan een klant dat eerder is opgehaald te verbinden. Iedere invulveld had verschillende Validators nodig die pastte bij de context van de attribuut van ieder object.
``` Typescript
// Inputform dat alle data opneemt van wat er is ingevult met regex op sommige numerieke invulvelden. Dit is gedaan op basis van de ontvangen informatie in een excel bestand van de opdrachtgever. Alle losstaande FormControls binnen de twee nested FormGroups zijn voor lees gemak in dit exemplaar verwijderd.
  inputData: FormGroup = new FormGroup({
    'articleData': new FormGroup({
    }),
    'articleDescription': new FormGroup({
    }),
    'customer': new FormControl(null, Validators.required)
  });
```
Het proces als de data binnen de forum valid is en vervolgens alles door de medewerker is ingevuld voor het versturen. Daarna worden entiteiten van ArtikelData en ArtikelDescriptie individueel doorgegeven naar de backend om een nieuwe id aan te maken. De volledige entiteiten worden pas ontvangen uit de Back-End als deze entiteiten zijn opgeslagen in de database. Vervolgens worden de ids van de twee entiteiten gebruikt voor het maken van de Artikel entiteit dat de klant en de artikel eigenschappen aan elkaar bindt met voornamelijk de ids. Deze gemaakte Artikel entiteit wordt dan uiteindelijk naar de database verstuurd om alle componenten aan elkaar te linken.
``` Typescript
// Het binden van de form met de eerder gemaakte variabelen en deze verwerken op basis van de entiteiten in de database.
  async onSubmit() {
    this.articleData = this.inputData.controls['articleData'].value;
    this.articleData.composition = this.inputData.get('articleData.composition.composition_percentage').value + "% " + this.inputData.get('articleData.composition.composition_category').value;
    this.articleDescription = this.inputData.controls['articleDescription'].value;
    this.customer = this.inputData.controls['customer'].value;

// Iedere helft van de Artikel wordt doorverstuurd om opgeslagen te worden. En opnieuw opgehaald uit de database met een automatisch gegenereerde Id uit de Back-End.
    const data = await firstValueFrom(this.http.sendData<ArticleData>("/api/v2/article_data", this.articleData));
    const desc = await firstValueFrom(this.http.sendData<ArticleDescription>("/api/v2/article_description", this.articleDescription));

    this.article.article_dataID = data;
    this.article.article_descriptionID = desc;
    this.articleWithCustomer.article = this.article;
    this.articleWithCustomer.customer = this.customer;
// Dan wordt de Artikel met de klant met een verzoek naar de Back-End opgeslagen in de database.
    this.http.sendData<ArticleCustomerRec>("/api/v2/article", this.articleWithCustomer);
// Maakt de inputForm leeg.
    this.onCancel();
  }
}
```

# De Technische Unie
Een elektronica winkel dat hulp nodig had bij het organiseren van de verlofdagen die werden gepland in een excel bestand. Echter vond het bedrijf deze aanpak onhandig en was er veel moeite in het goed bekijken of schema's van managers en medewerkers overlappen of niet. Het bedrijf vroeg voor een applicatie dat verlofaanvragen kon aanmaken en het doorleveren aan de managers binnen een bepaalde sectie. Vervolgens mogen de medewerkers alleen binnen hun eigen team alleen geaccepteerde verlofdagen zien, zonder dat andere teams het kunnen.

De onderstaande screenshot is een voorbeeld gemaakt uit pure HTML en CSS, voordat het project verder in productie werd genomen in Angular.
![Html concept](https://i.postimg.cc/Sxc4mJ6x/Schermafbeelding-2023-04-21-194824.png)

## VerlofKalenderpagina
De use-case voor deze scenario is het maken van een verlofkalender dat op basis van wie is ingelogd, vertoont wie er wanneer verlof heeft per dag in een weekkalender. Als een medewerker is ingelogd, mag die alleen de verlofdagen van zijn team zien. Maar de managers horen alle teams te kunnen zien. De kalender hoort één week in het verleden en in de toekomst kunnen navigeren. Uiteindelijk moest er de mogelijkheid zijn voor het terug navigeren naar de huidige week.

In deze applicatie kan een normale gebruiker niet al te veel zien behalve de originele kalender en een verlofverzoek versturen.
![Employee](https://i.postimg.cc/qqMVrzgY/Schermafbeelding-2023-12-25-224508.png)

Pas als er is ingelogd als een admin krijgt de gebruiker de extra menu opties te zien voor het aanmaken van entiteiten.
Voordat de applicatie iets in de kalender te verwerken horen er een paar medewerkers, teams en chapters klaar te staan voor gebruik. 
![Medewerker Toevoegen](https://i.postimg.cc/5NdNhmmQ/Schermafbeelding-2023-12-25-222755.png)
![Team Toevoegen](https://i.postimg.cc/yNWY06K0/Schermafbeelding-2023-12-25-222832.png)

Pas nadat een verlofverzoek van een medewerker is geaccepteerd, dan wordt het aangegeven binnen de kalender voor de periode waar het is aangegeven.
![Verlofverzoek](https://i.postimg.cc/ZYdVSt3Y/Schermafbeelding-2023-12-25-191907.png)

Daarnaast hebben alleen de admins toegang tot het accepteren van verlofverzoeken die worden verstuurd door medewerkers.
![VerlofAccepteren](https://i.postimg.cc/Yqy9fVng/Schermafbeelding-2023-12-25-192027.png)

De onderstaande screenshots zijn gemaakt om 17-12-2023, de dagen die geweest zijn horen automatisch donkergroen en grijze vlakken te hebben. Daarna moest de dagen in de toekomst lightgroen zijn om aan te geven dat ze nog aankomen. En uiteindelijk de huidige dag dat juist scherp geel en verschillend is in vergelijking met de dagen in de toekomst en verleden.
![Forum1](https://i.postimg.cc/9QBf3Z4C/Schermafbeelding-2023-12-17-230249.png)
![Forum2](https://i.postimg.cc/13WZcHnn/Schermafbeelding-2023-12-17-230228.png)
![Forum3](https://i.postimg.cc/C542MhbB/Schermafbeelding-2023-12-17-230327.png)

``` HTML
<div class="main-container d-flex">
  <div class="container shadow-lg">
    <div class="row">
    <!-- De maand boven aan de pagina wordt vertoond en navigatie knoppen om naar de volgende en vorige week te gaan -->
      <h2 class="text-center text-success mb-3 mt-3 .head-text">{{this.shownMonth | date: "MMMM"}} - {{this.shownMonth | date: "YYYY"}}</h2>
      <nav aria-label="Page navigation example">
        <ul class="pagination adjust-content">
          <li class="page-item">
            <button class="page-link" (click)="toPreviousWeek()">Previous</button>
          </li>
          <li class="page-item">
            <button class="page-link" (click)="toToday()">Today</button>
          </li>
          <li class="page-item">
            <button class="page-link" (click)="toNextWeek()" >Next</button>
          </li>
        </ul>
      </nav>
    </div>
```
In de onderstaande codestuk wordt de bovenste rij van tabs vertoond met de algemene data van de medewerkers die in dezelfde team zit. Daarnaast krijgt elk dag op basis van de huidige dag of een specifieke kleur.
``` HTML
    <div class="row">
      <table class="table table-striped">
        <thead>
        <tr>
          <th scope="col" class="row-title">Chapter</th>
          <th scope="col" class="row-title">Team</th>
          <th scope="col" class="row-title" style="border-right: 1px solid grey">Full Name</th>
          <th scope="col" class="calendar-table-centering"
              [ngStyle]="{
                'background-color': leaveRequestColoringService.previousDayDateStyling(day, this.curr),
                'border-color': leaveRequestColoringService.currDayBorder(day, this.curr),
                'color': leaveRequestColoringService.letterColering(day, this.curr)
                          }"
              [ngClass]="leaveRequestColoringService.currDayActive(day, this.curr)"
              *ngFor="let day of week">{{day | date: "d"}} - {{day | date: "E"}}</th>
        </tr>
        </thead>
```
In dit andere onderstaande code stuk is voor het inladen van eerder opgenomen data van meerdere medewerkers aan de hand van de persoon dat is ingelogd. In de eerste sectie van de rij komt de medewerkersdata in welk team en chapter ze tot behoren. Dan komt de volledige naam van de medewerker ernaast te staan. Ten slotte lege cellen plaatsen voor de kalender en ze individueel op basis van de medewerker's id en dag een eigen id geven. Deze id van een lege cel kan verwezen worden wanneer een medewerker op die dag verlof heeft.
``` HTML
        <tbody>
        <!-- Alle passende data invullen per rij per toegevoegde medewerker waarvan de verlofdag goedgekeurd is -->
        <tr *ngFor="let currentUser of userDTOs" id="row_{{currentUser.id}}">
          <td><div class="cell-content">{{currentUser.chapterName}}</div></td>
          <td><div class="cell-content">{{currentUser.teamName}}</div></td>
          <td style="border-right: 1px solid grey"><div class="cell-content">{{currentUser.fullName}}</div></td>
          <td class="calendar-table-centering" [ngStyle]="{'background-color': leaveRequestColoringService.previousDayCellStyling(day, this.curr)}" *ngFor="let day of week" id="{{currentUser.id + '_cell_' + day.getTime() }}"><div class="cell-content"></div></td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>
```
Aan het begin moet de applicatie de huidige week zien te krijgen en deze waarde stoppen in een sessiontoken voor gemak van de gebruiker. Echter door de internationale setting van het ophalen van een datum wordt het meestal gezet naar van Zondag naar Zaterdag, dit wordt ook aangepast naar Maandag tot Zondag. 
``` Typescript
/* Opzet van sessiontoken voor het correct opnemen van de huidige week en dag. */
  ngOnInit() {
    let curr = new Date
    this.checkForWindowSessionStorageToken()
    curr = this.dateService.setHourMinuteSecondMilliSecondToZero(curr)
    this.curr = this.dateService.setHourMinuteSecondMilliSecondToZero(this.curr)
    if (this.curr.getDay() == 0) {
      this.currentWeekNumber -= 1;
    }
/* het organiseren de dagen naar Mon - Sun, i.p.v. Sun - Sat */
    for (let i = 1; i <= 7; i++) {
      let first = curr.getDate() - curr.getDay() + i
      let day = new Date(curr.setDate(first))
      let insertableDay = new Date(day.getTime() + (this.currentWeekNumber * this.magicalWeekInMsNumber))
      this.week.push(insertableDay)
    }
    this.shownMonth = new Date(this.curr.getTime() + (this.currentWeekNumber * this.magicalWeekInMsNumber));
```
De onderstaande gedeelte wordt er gecontroleerd of de sessiontoken bestaat, als het bestaat dan wordt deze opgehaald. Voor het geval van niet wordt het aangemaakt en meteen opgeslagen.
``` Typescript
/* Het bewaren van de huidige week dat is berekend in een session token voor snellere navigatie in de calender.
 Deze token wordt ook gebruikt om één week in de toekomst, in de verleden en meteen terug te gaan de huidige week.*/
  private checkForWindowSessionStorageToken() {
    let stringedRetrievedWeekNumber;
    if (!(window.sessionStorage.hasOwnProperty('currentWeekNumber'))) {
      window.sessionStorage.setItem('currentWeekNumber', JSON.stringify(0));
      this.currentWeekNumber = 0;
    } else {
      stringedRetrievedWeekNumber = window.sessionStorage.getItem('currentWeekNumber');
      if (stringedRetrievedWeekNumber != null) {
        this.currentWeekNumber = JSON.parse(stringedRetrievedWeekNumber);
      }
    }
  }
```
Vanuit een service worden eerst alle gebruikers opgehaald, van op basis van de gebruikers kijken welke geaccepteerde verlofdagen ze hebben. Nadat deze informatie is opgehaald, dan worden de chapters en teams gebonden opgehaald van de medewerkers. En uiteindelijk met het verzamelde data wordt alle verlofdagen op basis van de huidige week vertoond met medewerker gegevens ingevuld in de lege cellen van de kalender.
``` Typescript
/* Alle medewerkers in de aangegeven team verbinden aan hun chapters en verlofverzoeken die zijn geaccepteerd in de huidige week dat in de session token zit. */
    this.userService.getAllUsers().subscribe({
      next: () => {
        this.collectedUserData = this.userService.users;
        this.leaverequestService.getLeaveRequests().subscribe({
          next: () => {
            this.collectedAcceptedLeaveRequests = this.leaverequestService.leaveRequests;
            this.chapterService.getAllChapters().subscribe({
              next: () => {
                this.collectedChapters = this.chapterService.chapters;
                this.teamService.getTeams().subscribe({
                  next: () => {
                    this.collectedTeams = this.teamService.teams;
                    this.userDTOs = this.getUserData();
                    this.getCalenderData()
                      .then(e=> {
                          this.currentWeek = e
                          this.processCalenderCells(this.currentWeek);
                        }
                      );
                  }
                });
              }
            });
          }
        });
      }
    });
  }

```
Hier worden de elk medewerker één voor één gebonden aan hun bijbehorende chapter en team binnen de organisatie. Daarna wordt alleen de benodige informatie dat in de kalender wordt vertoond verder doorgeleverd in de applicatie.
``` Typescript
/* Ophalen van alle medewerkergegevens die in dezelfde team als degene die is ingelogd, echter isoleert het de andere teams die niet tot behoren. De managers krijgen toegang tot elk team in de verlofkalender. */
  private getUserData() {
    let shortenedUserList: UserDTOWithNames[] = []
    for (let user of this.userService.users) {
      let boundTeamNameToCurrentUser = "-";
      let boundChapterNameToCurrentUser = "-";
      for (let chapter of this.chapterService.chapters) {
        if (user.chapterId == chapter.id) {
          boundChapterNameToCurrentUser = chapter.name;
        }
      }
      for (let team of this.teamService.teams) {
        if (user.teamId == team.id) {
          boundTeamNameToCurrentUser = team.name;
        }
      }
      if (user.id != null) {
        shortenedUserList.push(new UserDTOWithNames(user.id, user.fullName, boundTeamNameToCurrentUser, boundChapterNameToCurrentUser))
      }
    }
    return shortenedUserList;
  }
```
Medewerkers die worden opgehaald verbinden aan de gaccepteerde verlofaanvragen binnen de kalender. Hiervan worden alleen de relevanten stukken van data opgehaald en naast de bijbehorende medewerker geplaatst.
``` Typescript
/* het benodigde informatie ophalen van de geaccepteerde verlofverzoeken die relevant zijn voor het invullen van de kalender per dag. */
  private async getCalenderData(): Promise<{ userId: string, leaveRequestDTO: LeaveRequestDTO[] }[]> {
    let leaveRequestDTOList: { userId: string, leaveRequestDTO: LeaveRequestDTO[] }[] = [];
    let res = await firstValueFrom(this.leaverequestService.getAcceptedLeaveRequestDTO())

      for (let leaveRequestDTO of res) {
        if (leaveRequestDTO.leaveRequestId != null) {
          leaveRequestDTOList.push(
            {
              userId: leaveRequestDTO.userId,
              leaveRequestDTO: [
                new LeaveRequestDTO(
                  leaveRequestDTO.leaveRequestId,
                  leaveRequestDTO.startDate,
                  leaveRequestDTO.endDate,
                  leaveRequestDTO.typeRequest,
                  leaveRequestDTO.userId
                )
              ]
            }
          )
        }
      }
    return await new Promise<{userId: string; leaveRequestDTO: LeaveRequestDTO[]}[]>(resolve => {
      resolve(leaveRequestDTOList);
    });
  }
```
De onderstaande method is bedoeld voor het oppakken van de eerder vertoonde lege cellen met een eigen id en deze invullen als er een verlofperiode op die dag zit. Dit wordt gedaan op basis van de eind en begin datums van elk verlof dat is opgenomen van de medewerkers die zijn geaccepteerd.
``` Typescript
/* Het aanpassen van de kalender in HTML op basis van id met de opgenomen data */
  public processCalenderCells(data: { userId: string, leaveRequestDTO: LeaveRequestDTO[] }[]) {
    for (const userLeaveRequests of data) {
      let targetableId = userLeaveRequests.userId + "_cell_";
      for (const userData of userLeaveRequests.leaveRequestDTO) {
        let tempTime = userData.startDate;
        while (tempTime <= userData.endDate) {
          let dateInMs = tempTime.getTime();
          let htmlId = targetableId + dateInMs;
          let htmlElement = document.getElementById(htmlId);
          if (htmlElement != null) {
            let childHtmlElement = htmlElement.firstElementChild;
            if (childHtmlElement != null)
              childHtmlElement.classList.add("background-checkered");
          }
          tempTime.setDate(tempTime.getDate() + 1);
        }

      }
    }
  }
}
```

