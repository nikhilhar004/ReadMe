# ReadMe
Paar exemplaren van code binnen privé repositories van Hogeschool Leiden.

# Toevoegingsforum voor de Gordijnrollenbeheersysteem voor Atelier van der Lelie
```HTML
<h2 mat-dialog-title>Afval Toevoegen</h2>
<form (ngSubmit)="onSubmit()" [formGroup]="inputData">
  <mat-dialog-content>
    <mat-grid-list cols="3" rowHeight="75px">
      <!-- Eerste helft van de Object Artikel in het systeem genaamd ArticleData -->
      <div formGroupName="articleData">
        <mat-grid-tile colspan="1">
          <mat-form-field >
            <mat-label>Kleur</mat-label>
            <input matInput type="text" name="color" id="color" formControlName="color">
            <mat-error *ngIf="!inputData.get('articleData.color').valid && inputData.get('articleData.color').touched">Kleur is <b>verplicht</b></mat-error>
          </mat-form-field>
        </mat-grid-tile>
        <mat-grid-tile colspan="1">
          <mat-form-field>
            <!-- herhaling van andere benodigde attributen voor het toevoegen van een Artikel, productgroup & supplier-->
          </mat-form-field>
        </mat-grid-tile>
        <div formGroupName="composition">
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
        <mat-grid-tile>
          <mat-form-field>
            <mat-label>Compositie stof</mat-label>
            <mat-select id="composition_category" formControlName="composition_category">
              <mat-option
                *ngFor="let requirement_category of requirement_categories; let i = index" [value]="requirement_category">
                {{ requirement_category }}
              </mat-option>
            </mat-select>
            <mat-error *ngIf="!inputData.get('articleData.composition.composition_category').valid && inputData.get('articleData.composition.composition_category').touched">Compositie is <b>verplicht</b></mat-error>
          </mat-form-field>
        </mat-grid-tile>
      </div>
        <mat-grid-tile>
          <mat-form-field>
            <mat-label>Patroon Lengte</mat-label>
            <input matInput type="number" name="pattern_length" id="pattern_length" formControlName="patternLength">
            <mat-error *ngIf="!inputData.get('articleData.patternLength').valid && inputData.get('articleData.patternLength').value < 0 && inputData.get('articleData.patternLength').value != null">Patroon L. mag niet onder <b>0</b> zitten</mat-error>
            <mat-error *ngIf="!inputData.get('articleData.patternLength').valid && inputData.get('articleData.patternLength').touched">Patroon Lengte is <b>verplicht</b></mat-error>
          </mat-form-field>
        </mat-grid-tile>
        <mat-grid-tile>
          <!-- pattern width-->
        </mat-grid-tile>

        <mat-grid-tile>
          <mat-form-field>
            <mat-label>Is een Voorraad Artikel</mat-label>
            <mat-select formControlName="stockRL">
              <mat-option
                *ngFor="let stockRLoption of stockRL.ifStockIsArticle; let i = index" [value]="stockRL.stockRLValues[i]">
                {{ stockRLoption }}
              </mat-option>
            </mat-select>
            <mat-error *ngIf="!inputData.get('articleData.stockRL').valid && inputData.get('articleData.stockRL').touched">Aangeven of dit een Voorraad Artikel is <b>verplicht</b></mat-error>
          </mat-form-field>
        </mat-grid-tile>
        <mat-grid-tile>
          <!-- eancode-->
        </mat-grid-tile>
      </div>
      <!-- Tweede helft van het Object Artikel genaamd ArticleDescription -->
      <div formGroupName="articleDescription">
        <mat-grid-tile>
          <!-- layout-->
        </mat-grid-tile>

        <mat-grid-tile>
          <mat-form-field>
            <mat-label>Gewicht</mat-label>
            <input matInput type="number" name="weight" id="weight" formControlName="weight">
            <mat-error *ngIf="!inputData.get('articleDescription.weight').valid && inputData.get('articleDescription.weight').value <= 0 && inputData.get('articleDescription.weight').value != null">Gewicht mag niet onder <b>1</b> zitten</mat-error>
            <mat-error *ngIf="!inputData.get('articleDescription.weight').valid && inputData.get('articleDescription.weight').touched">Gewicht is <b>verplicht</b></mat-error>
          </mat-form-field>
        </mat-grid-tile>

        <mat-grid-tile>
          <!-- washcode-->
        </mat-grid-tile>
        <mat-grid-tile>
          <!-- tiltable -->
        </mat-grid-tile>

        <mat-grid-tile>
          <!-- articlenummer -->
        </mat-grid-tile>
        <mat-grid-tile>
          <!-- type-->
        </mat-grid-tile>
        <mat-grid-tile>
          <mat-form-field>
            <mat-label>Stof Breedte</mat-label>
            <input matInput type="number" name="cloth_width" id="cloth_width" formControlName="clothWidth">
            <mat-error *ngIf="!inputData.get('articleDescription.clothWidth').valid && inputData.get('articleDescription.clothWidth').value <= 0 && inputData.get('articleDescription.clothWidth').value != null">StofBreedte mag niet onder <b>1</b> zitten</mat-error>
            <mat-error *ngIf="!inputData.get('articleDescription.clothWidth').valid && inputData.get('articleDescription.clothWidth').touched">Stof Breedte is <b>verplicht</b></mat-error>
          </mat-form-field>
        </mat-grid-tile>

        <mat-grid-tile>
          <mat-form-field>
            <mat-label>Minimale Voorraad</mat-label>
            <input matInput type="number" name="minimum_stock" id="minimum_stock" formControlName="minimumStock">
            <mat-error *ngIf="!inputData.get('articleDescription.minimumStock').valid && inputData.get('articleDescription.minimumStock').value < 0 && inputData.get('articleDescription.minimumStock').value != null">Min. Voorraaad mag niet onder <b>0</b> zitten</mat-error>
            <mat-error *ngIf="!inputData.get('articleDescription.minimumStock').valid && inputData.get('articleDescription.minimumStock').touched">Minimale Voorraad is <b>verplicht</b></mat-error>
          </mat-form-field>
        </mat-grid-tile>
        <mat-grid-tile>
          <!-- description -->
        </mat-grid-tile>
      </div>
      <mat-grid-tile>
        <mat-form-field appearance="fill">
          <mat-label>Klant</mat-label>
          <mat-select formControlName="customer">
            <mat-option *ngFor="let customer of customers" id="customer" [value]="customer" >
              {{ customer.name }}
            </mat-option>
          </mat-select>
          <mat-error *ngIf="!inputData.get('customer').valid && inputData.get('customer').touched">Klant is <b>verplicht</b></mat-error>
        </mat-form-field>
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
```Typescript
import {Component, OnInit} from '@angular/core';
import {FormControl, FormGroup, Validators} from "@angular/forms";
import {Article} from "../../models/article.model";
import {HttpService} from "../../../../services/http.service";
import {ArticleData} from "../../models/article-data.model";
import {ArticleDescription} from "../../models/article-description.model";
import {firstValueFrom} from "rxjs";
import {Customer} from "../../../core/models/customer.model";
import {ArticleCustomerRec} from "../../models/ArticleCustomerRec.model";


@Component({
  selector: 'app-waste-add',
  templateUrl: './waste-add.component.html',
  styleUrls: ['./waste-add.component.scss']
})
export class WasteAddComponent implements OnInit{
  constructor(private http: HttpService) {

  }

  ngOnInit() {
    this.getCustomers()
  }

  private articleData = new ArticleData;
  private articleDescription = new ArticleDescription;
  private article = new Article;
  private articleWithCustomer: ArticleCustomerRec = new ArticleCustomerRec;
  private customer = new Customer;

  wasteDataID = 0
  wasteDescripton = 0
  customers: Customer[];

  requirement_categories: string[] = [
    'CO',
    'PL',
    'PES',
    'LI',
    'TREVIRA'
  ]

  not_tiltable = {
    'ableToTilt': ['Kantelbaar', 'Onkantelbaar'],
    'tiltValues': [false, true]
  }

  stockRL = {
    'ifStockIsArticle': ['Ja', 'Nee'],
    'stockRLValues': [true, false]
  }

  inputData: FormGroup = new FormGroup({
    'articleData': new FormGroup({
      'color': new FormControl(null, Validators.required),
      'productgroup': new FormControl(null, Validators.required),
      'supplier': new FormControl(null, Validators.required),
      'patternWidth': new FormControl(null, [Validators.required, Validators.pattern(/^[0-9]+[0-9]*$/)]),
      'patternLength': new FormControl(null, [Validators.required, Validators.pattern(/^[0-9]+[0-9]*$/)]),
      'composition': new FormGroup({
        'composition_percentage': new FormControl(null, [Validators.required, Validators.pattern(/^[1-9][0-9]?$|^100$/)]),
        'composition_category': new FormControl(null, Validators.required)
      }),
      'eancode': new FormControl(null, Validators.required),
      'stockRL': new FormControl(null, Validators.required)
    }),
    'articleDescription': new FormGroup({
      'weight': new FormControl(null, [Validators.required, Validators.pattern(/^[1-9]+[0-9]*$/)]),
      'layout': new FormControl(null, Validators.required),
      'washcode': new FormControl(null, Validators.required),
      'type': new FormControl(null, Validators.required),
      'not_tiltable': new FormControl(null, Validators.required),
      'articlenumber': new FormControl(null, Validators.required),
      'clothWidth': new FormControl(null, [Validators.required, Validators.pattern(/^[1-9]+[0-9]*$/)]),
      'minimumStock': new FormControl(null, [Validators.required, Validators.pattern(/^[0-9]+[0-9]*$/)]),
      'description': new FormControl(null, Validators.required)
    }),
    'customer': new FormControl(null, Validators.required)
  });

  getCustomers() {
    return this.http.getData<Customer>("/api/v2/customer").subscribe(
      (_customerList) => {
        this.customers = _customerList
      }
    );
  }

  async onSubmit() {
    this.articleData = this.inputData.controls['articleData'].value;
    this.articleData.composition = this.inputData.get('articleData.composition.composition_percentage').value + "% " + this.inputData.get('articleData.composition.composition_category').value;
    this.articleDescription = this.inputData.controls['articleDescription'].value;
    this.customer = this.inputData.controls['customer'].value;

    const data = await firstValueFrom(this.http.sendData<ArticleData>("/api/v2/article_data", this.articleData));
    const desc = await firstValueFrom(this.http.sendData<ArticleDescription>("/api/v2/article_description", this.articleDescription));

    this.article.article_dataID = data;
    this.article.article_descriptionID = desc;
    this.articleWithCustomer.article = this.article;
    this.articleWithCustomer.customer = this.customer;
    console.log(JSON.stringify(this.articleWithCustomer));
    this.http.sendData<ArticleCustomerRec>("/api/v2/article", this.articleWithCustomer);

    this.onCancel();
  }

  onCancel() {
    this.inputData.reset();
  }

}
```

# VerlofCalenderpagina voor de Technische Unie
``` HTML
<app-navigation></app-navigation>
<app-toasts></app-toasts>
<div class="main-container d-flex">
  <div class="container shadow-lg">
    <div class="row">
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
        <tbody>
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
<app-footer></app-footer>
```
``` SCSS
body {
  background-color: #e5e8d6;
}

html, body { height: 100%; }
body { margin: 0; font-family: Roboto, "Helvetica Neue", sans-serif; }

.main-container {
  min-height: 100vh;
  background-color: #e5e8d6;
}

.container {
  background-color: white;
  overflow-y: scroll;
}

.calendar-table-centering {
  width: 10%;
  .cell-content {
    aspect-ratio: 3 / 2;
  }
}

.active {
  background-color: #f8dc0a;
}

.row-title {
  background-color: #3d9152;
  color: #EAFFF5;
}

.adjust-content {
  justify-content: end;
}

td {
  border-right: 1px solid lightgrey;
}

th {
  text-align: center;
  position: sticky;
  top: 0;
}

.background-checkered {
  opacity: 0.5;
  background: repeating-linear-gradient( 45deg, #444cf7, #444cf7 5px, #e5e5f7 5px, #e5e5f7 25px );
  border: #444cf7 solid 1px;
  border-radius: 5px;

}

@media screen and (max-width: 600px) {
  h2 {
    font-size: 30px;
  }

  td > * {
    font-size: 9px;
  }

  .row-title {
    font-size: 9px;
  }

  button {
    font-size: 15px;
  }

  .main-container {
    min-height: 81vh;
  }

  .calendar-table-centering {
    font-size: 9px;
    .cell-content {
      aspect-ratio: auto;
      min-height: 40px;
      width: 100%;
    }
  }

  .background-checkered {
    opacity: 0.5;
    background: repeating-linear-gradient(45deg, #444cf7, #444cf7 5px, #e5e5f7 5px, #e5e5f7 25px);
    border: #444cf7 solid 1px;
    border-radius: 5px;
  }

  .adjust-content {
    justify-content: center;
  }
}
```
``` Typescript
import {Component, OnInit} from '@angular/core';
import {UserDTOWithNames} from "../shared/models/UserDTOWithNames.model";
import {LeaveRequestDTO} from "../shared/models/LeaveRequestDTO.model";
import {LeaveRequestColeringService} from "../shared/services/LeaveRequestColering.service";
import {UserService} from "../shared/services/user.service";
import {User} from "../shared/models/User.model";
import {LeaveRequestService} from "../shared/services/LeaveRequest.service";
import {LeaveRequest} from "../shared/models/LeaveRequest.model";
import {TeamService} from "../shared/services/Team.service";
import {ChapterService} from "../shared/services/chapter.service";
import {Team} from "../shared/models/Team.model";
import {Chapter} from "../shared/models/Chapter.model";
import {DateService} from "../shared/services/Date.service";
import {firstValueFrom} from "rxjs";

@Component({
  selector: 'app-leaverequest-overview',
  templateUrl: './leaverequest-overview.component.html',
  styleUrls: ['./leaverequest-overview.component.scss']
})
export class LeaverequestOverviewComponent implements OnInit {

  constructor(
    public leaveRequestColoringService: LeaveRequestColeringService,
    private leaverequestService: LeaveRequestService,
    private userService: UserService,
    private teamService: TeamService,
    private chapterService: ChapterService,
    private dateService: DateService
  ) {
  }

  public curr = new Date();
  public week: Date[] = [];
  public currentWeekNumber: number;
  public currentWeek: { userId: string, leaveRequestDTO: LeaveRequestDTO[] }[] = []
  public shownMonth = new Date();
  public magicalWeekInMsNumber = 7 * 24 * 60 * 60 * 1000;
  public userDTOs: UserDTOWithNames[] = [];
  private collectedUserData: User[] = [];
  private collectedAcceptedLeaveRequests: LeaveRequest[] = [];
  private collectedTeams: Team[] = [];
  private collectedChapters: Chapter[] = [];


  ngOnInit() {
    let curr = new Date
    this.checkForWindowSessionStorageToken()
    curr = this.dateService.setHourMinuteSecondMilliSecondToZero(curr)
    this.curr = this.dateService.setHourMinuteSecondMilliSecondToZero(this.curr)
    if (this.curr.getDay() == 0) {
      this.currentWeekNumber -= 1;
    }
    for (let i = 1; i <= 7; i++) {
      let first = curr.getDate() - curr.getDay() + i
      let day = new Date(curr.setDate(first))
      let insertableDay = new Date(day.getTime() + (this.currentWeekNumber * this.magicalWeekInMsNumber))
      this.week.push(insertableDay)
    }
    this.shownMonth = new Date(this.curr.getTime() + (this.currentWeekNumber * this.magicalWeekInMsNumber));
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

  public toNextWeek() {
    let showedWeekNumber;
    if (window.sessionStorage.getItem('currentWeekNumber') != null) {
      showedWeekNumber = window.sessionStorage.getItem('currentWeekNumber')
      if (showedWeekNumber != null) {
        showedWeekNumber = JSON.parse(showedWeekNumber);
        showedWeekNumber += 1;
        window.sessionStorage.removeItem('currentWeekNumber');
        window.sessionStorage.setItem('currentWeekNumber', showedWeekNumber)
      }
    }
    window.location.reload();
  }

  public toToday() {
    window.sessionStorage.removeItem('currentWeekNumber');
    window.sessionStorage.setItem('currentWeekNumber', JSON.stringify(0));
    window.location.reload();
  }

  public toPreviousWeek() {
    let showedWeekNumber;
    if (window.sessionStorage.getItem('currentWeekNumber') != null) {
      showedWeekNumber = window.sessionStorage.getItem('currentWeekNumber')
      if (showedWeekNumber != null) {
        showedWeekNumber = JSON.parse(showedWeekNumber);
        showedWeekNumber -= 1;
        window.sessionStorage.removeItem('currentWeekNumber');
        window.sessionStorage.setItem('currentWeekNumber', showedWeekNumber)
      }
    }
    window.location.reload();
  }
}
```
