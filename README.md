# ReadMe
Paar exemplaren van code binnen privé repositories van Hogeschool Leiden.

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
