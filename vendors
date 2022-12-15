import {Component, EventEmitter, Input, OnInit, Output, OnDestroy} from '@angular/core';
import {isNullOrUndefined} from "util";
import {User} from '../../../models/user';
import {Listing} from "../../../models/listing";
import {getContactMaintenanceCatagoryType} from "../../../utils";
import {ListingVendorsMaintenancesPopupComponent} from "../../listing/popups/listing-vendors-maintenances";
import {MatDialog, MatDialogRef} from "@angular/material";
import {getIsVendorsLoaded, getIsVendorsLoading, getVendors, State} from "../../../reducers/index";
import {Observable} from "rxjs/Observable";
import {StayDuvetService} from "../../../services/stayduvet";
import {Store} from "@ngrx/store";

@Component({
  selector: 'sd-approve-listing-popup',
  template: `
    <sd-modal-popup-layout title="{{title}}" *ngIf="showApproveListingUI">
      <div  style="padding-top: 30px;" fxLayout="column" fxLayoutAlign="center stretch" >
      
        <div fxLayout="row">
          <mat-form-field fxFill>
            <mat-select
              placeholder="Assign a Assignee for this Property"
              [(ngModel)]="selectedAssgineeId"
              (ngModelChange)="onAssigneeSelected()"
              color="accent"
              [ngModelOptions]="{standalone: true}">
              <mat-option *ngFor="let user of users" [value]="user.getAdmin().id">
                {{user.first_name}} {{user.last_name}}
              </mat-option>
            </mat-select>
          </mat-form-field>
        </div>
        <mat-error *ngIf="assigneeError" style="font-size: 15px;">{{assigneeError}}</mat-error>
      
        <div fxLayout="row">
          <mat-form-field fxFill>
            <mat-select
              placeholder="Assign a Housekeeper for this Property"
              [(ngModel)]="selectedHouseKeeperId"
              (ngModelChange)="onHouseKeeperSelected()"
              color="accent"
              [ngModelOptions]="{standalone: true}">
              <mat-option *ngFor="let user of getVendor('housekeeper')" [value]="user.getManagementContact().id">
                {{user.first_name}} {{user.last_name}}
              </mat-option>
            </mat-select>
          </mat-form-field>
        </div>
        <mat-error *ngIf="houseKeeperError" style="font-size: 15px;">{{houseKeeperError}}</mat-error>
        
        <div fxLayout="row">
          <mat-form-field fxFill>
          <mat-select
            placeholder="Assign a General Maintenance for this Property"
            [(ngModel)]="selectedGeneralMaintenanceId"
            color="accent"
            [ngModelOptions]="{standalone: true}">
            <mat-option *ngFor="let user of getVendor('general_maintenance')" [value]="user.getManagementContact().id">
              {{user.first_name}} {{user.last_name}}
            </mat-option>
          </mat-select>
          </mat-form-field>
        </div>
        
        <div fxLayout="row">
          <mat-form-field fxFill>
          <mat-select
            placeholder="Assign a Hvac for this Property"
            [(ngModel)]="selectedHvacId"
            color="accent"
            [ngModelOptions]="{standalone: true}">
            <mat-option *ngFor="let user of getVendor('hvac')" [value]="user.getManagementContact().id">
              {{user.first_name}} {{user.last_name}}
            </mat-option>
          </mat-select>
          </mat-form-field>
        </div>
        
        <div fxLayout="row">
          <mat-form-field fxFill>
              <mat-select
                placeholder="Assign a Plumber for this Property"
                [(ngModel)]="selectedPlumberId"
                color="accent"
                [ngModelOptions]="{standalone: true}">
                <mat-option *ngFor="let user of getVendor('plumber')" [value]="user.getManagementContact().id">
                  {{user.first_name}} {{user.last_name}}
                </mat-option>
              </mat-select>
          </mat-form-field>
        </div>
        
        <div fxLayout="row">
          <mat-form-field fxFill>
            <mat-select
              placeholder="Assign a Painter for this Property"
              [(ngModel)]="selectedPainterId"
              color="accent"
              [ngModelOptions]="{standalone: true}">
              <mat-option *ngFor="let user of getVendor('painter')" [value]="user.getManagementContact().id">
                {{user.first_name}} {{user.last_name}}
              </mat-option>
            </mat-select>
          </mat-form-field>
        </div>
        
        <div fxLayout="row">
          <mat-form-field fxFill>
            <mat-select
              placeholder="Assign a Electrician for this Property"
              [(ngModel)]="selectedElectricianId"
              color="accent"
              [ngModelOptions]="{standalone: true}">
              <mat-option *ngFor="let user of getVendor('electrician')" [value]="user.getManagementContact().id">
                {{user.first_name}} {{user.last_name}}
              </mat-option>
            </mat-select>
          </mat-form-field>
        </div>
        
        <div fxLayout="row">
          <mat-form-field fxFill>
            <mat-select
              placeholder="Assign a Cleaner for this Property"
              [(ngModel)]="selectedCleanerId"
              color="accent"
              [ngModelOptions]="{standalone: true}">
              <mat-option *ngFor="let user of getVendor('cleaner')" [value]="user.getManagementContact().id">
                {{user.first_name}} {{user.last_name}}
              </mat-option>
            </mat-select>
          </mat-form-field>
        </div>
        
        <div fxLayout="row">
          <mat-form-field fxFill>
            <mat-select
              placeholder="Assign a Homeowner for this Property"
              [(ngModel)]="selectedHomeOwnerId"
              color="accent"
              [ngModelOptions]="{standalone: true}">
              <mat-option *ngFor="let user of getVendor('homeowner')" [value]="user.getManagementContact().id">
                {{user.first_name}} {{user.last_name}}
              </mat-option>
            </mat-select>
          </mat-form-field>
        </div>
        

      
        <div fxLayout="row" fxLayoutAlign="end center" fxLayoutGap="10px">
          <button mat-raised-button color="accent" (click)="onAddContact()">Add Contact</button>
          <button mat-raised-button color="accent" [disabled]="isLoading" (click)="onYesButtonClicked()">Approve</button>
          <mat-spinner *ngIf="isLoading" [diameter]="30" [strokeWidth]="4"></mat-spinner>
       </div>
      </div>
    </sd-modal-popup-layout>
    
    <sd-create-contact-popup *ngIf="showAddContact" [listing]="listing">
      <div *ngIf="showBackButton" fxFlex="100*" fxLayoutAlign="start start">
        <button (click)="backButton()" mat-raised-button color="accent" >
          <mat-icon>keyboard_backspace</mat-icon>
          Back
        </button>
      </div>
    </sd-create-contact-popup>
    
  `,
  styles: [`
    mat-spinner {
      width: 24px;
      height: 24px;
      margin-right: 20px;
    }
  `]
})

export class ApproveListingPopupComponent implements OnInit, OnDestroy {

  @Input() isLoading: boolean = false;
  @Input() title: string = '';
  @Output() yesButtonClicked = new EventEmitter;
  selectedAssgineeId: number;
  selectedHouseKeeperId: number;
  selectedGeneralMaintenanceId: number;
  selectedHvacId: number;
  selectedPlumberId: number;
  selectedPainterId: number;
  selectedElectricianId: number;
  selectedCleanerId: number;
  selectedHomeOwnerId: number;

  showBackButton: boolean = false;
  showAddContact: boolean = false;
  showApproveListingUI: boolean = true;


  @Input() users: User[];

  dialogRef: MatDialogRef<any>;
  houseKeeperError: string =null;
  assigneeError: string = null;


  @Input() listing: Listing = {} as Listing;

  vendors: User[] = [];
  allVendors: User[] = [];


  vendorsLoading = false;
  vendorsLoaded = false;
  isAlive: boolean = true;


  ngOnInit() {
    this.vendors = this.listing.getMaintenancesContacts();
    this.selectedAssgineeId = this.listing.assignee_id;


    const housekeepers = this.vendors.filter((vendor: User) => vendor['managementContact']['data'].category == 'housekeeper');
    const maintenances = this.vendors.filter((vendor: User) => vendor['managementContact']['data'].category == 'general_maintenance');
    const hvac = this.vendors.filter((vendor: User) => vendor['managementContact']['data'].category == 'hvac');
    const painters = this.vendors.filter((vendor: User) => vendor['managementContact']['data'].category == 'painter');
    const plumbers = this.vendors.filter((vendor: User) => vendor['managementContact']['data'].category == 'plumber');
    const electricians = this.vendors.filter((vendor: User) => vendor['managementContact']['data'].category == 'electrician');
    const homeowners = this.vendors.filter((vendor: User) => vendor['managementContact']['data'].category == 'homeowner');
    const cleaners = this.vendors.filter((vendor: User) => vendor['managementContact']['data'].category == 'cleaner');

    if (housekeepers.length > 0) {

      this.selectedHouseKeeperId = housekeepers[0]['managementContact']['data'].id;
    }

    if (maintenances.length > 0) {
      this.selectedGeneralMaintenanceId = maintenances[0]['managementContact']['data'].id;
    }

    if (hvac.length > 0) {
      this.selectedHvacId = hvac[0]['managementContact']['data'].id;
    }
    if (painters.length > 0) {
      this.selectedPainterId = painters[0]['managementContact']['data'].id;
    }
    if (plumbers.length > 0) {
      this.selectedPlumberId = plumbers[0]['managementContact']['data'].id;
    }
    if (electricians.length > 0) {
      this.selectedElectricianId = electricians[0]['managementContact']['data'].id;
    }
    if (homeowners.length > 0) {
      this.selectedHomeOwnerId = homeowners[0]['managementContact']['data'].id;
    }
    if (cleaners.length > 0) {
      this.selectedCleanerId = cleaners[0]['managementContact']['data'].id;
    }

    this.selectedAssgineeId = this.listing.assignee_id;
    this.setUpVendors();
  }

  constructor(private dialog: MatDialog, private service: StayDuvetService, private store: Store<State>,) {

  }

  onYesButtonClicked() {
    if (isNullOrUndefined(this.selectedAssgineeId)) {
      this.assigneeError = 'This field is required';
      return;
    }

    if (isNullOrUndefined(this.selectedHouseKeeperId)) {
      this.houseKeeperError = 'This field is required';
      return;
    }


    this.yesButtonClicked.emit({
      assignee_id: this.selectedAssgineeId,
      housekeeper_id: this.selectedHouseKeeperId,
      general_maintenance_id: this.selectedGeneralMaintenanceId,
      painter_id: this.selectedPainterId,
      electrician_id: this.selectedElectricianId,
      plumber_id: this.selectedPlumberId,
      homeowner_id: this.selectedHomeOwnerId,
      cleaner_id: this.selectedCleanerId,
      hvac_id: this.selectedHvacId,
    });
  }

  getCategory(slug: string): string {
    return getContactMaintenanceCatagoryType(slug).title;
  }


  onAssigneeSelected() {
    this.assigneeError = null;
  }

  onHouseKeeperSelected() {
    this.houseKeeperError = null;
  }

  setUpVendors() {
    this.store.select(getIsVendorsLoading).takeWhile(() => this.isAlive).subscribe((loading) => {
      this.vendorsLoading = loading;
    });
    this.store.select(getIsVendorsLoaded).takeWhile(() => this.isAlive).subscribe((loaded) => {
      this.vendorsLoaded = loaded;
    });
    this.store.select(getVendors).takeWhile(() => this.isAlive).subscribe((vendors) => {
      if (vendors.length > 0) {
       this.allVendors = vendors
      }
    });

    const mergedObservables = Observable.merge(
      this.store.select(getIsVendorsLoading),
      this.store.select(getIsVendorsLoaded),
      this.store.select(getVendors),
      ((breakdown, loading, loaded) => {
      })
    );

    mergedObservables.takeWhile(() => this.isAlive).subscribe(
      (data) => {
        if (!this.vendorsLoading && !this.vendorsLoaded) {
          this.service.getVendors().subscribe();
        }
      });
  }

  getVendor(category)
  {
    console.log(category);
    if(this.allVendors.length > 0)
    {
      return this.allVendors.filter(contact => contact.managementContact['data'].category == category);
    }
    return [];
  }

  ngOnDestroy(): void {

    this.isAlive = false;
  }

  onAddContact()
  {
    this.showBackButton = true;
    this.showAddContact = true;
    this.showApproveListingUI = false;
  }

  backButton()
  {
    this.showApproveListingUI = true;
    this.showAddContact = false;
    this.showBackButton = false;
  }
}
