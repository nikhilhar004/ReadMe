# ReadMe
Paar exemplaren van code binnen privé repositories van Hogeschool Leiden.

# Atelier van der Lelie
Samen met een groep van studenten moest ik als een team een applicatie maken voor het bedrijf Atelier van der Lelie. De algemene opdracht was een applicatie te maken dat kon helpen met de onderhoud en beheer van een oude netwerk van deze organisatie. In deze organisatie wordt textiel en maatwerk producten maken van ondermaten, echter levert dit restanten op die aan opdrachtgevers van deze organisatie wordt geleverd. Meestal wilden een kleine percentage van deze opdrachtgevers de restanten terug krijgen. Het organiseren van de restanten en bepalen of de opdrachtgevers deze restanten terug willen krijgen, het bedrijf vroeg voor een applicatie die daarmee kon helpen aan de team.

## Toevoegingsforum voor de Gordijnrollenbeheersysteem in Angular
De use case voor deze functionaliteit is, dat er een methode nodig was om een nieuwe artikel van ondermaten toe te voegen aan een database die door andere leden van de team is opgezet. Deze database was gebaseerd op alleen informatie dat was gegeven in een excel bestand, de waardes die aanwezig waren niet al te concreet. Hierdoor moesten ik en de team waarmee ik werkte op basis van de entiteiten de gegeven van de artikel dat wordt doorgegeven opsplitsen. Dit werd gedaan in twee delen samen met één klant die gebonden wordt aan de bestelling, de indelingen van objecten zijn:
```
  -Artikel,
  -ArtikelData,
  -ArtikelDescriptie,
  -Klant,
```
Dit moest volgens de onderstaande figma exemplaar die was gemaakt, voordat dit project werd ontwikkeld in Angular.
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

# VerlofCalenderpagina voor de Technische Unie
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
``` Typescript
/* Ophalen van alle medewerkergegevens die in dezelfde team als degene die is ingelogd, echter isoleert het de andere teams die niet tot behoren */
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
