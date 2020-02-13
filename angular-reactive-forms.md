**Angular Reactive Forms by Deborah Kurata - Pluralsight**
1. Template-driven
    - manage data in HTML tempalte
    - form model created automatically by Angular (form is a FormGroup, ngModel-labeled inputs are FormControls etc.)
    - FormsModule : ngForm, ngModel, ngModelGroup
- Validation
```
<form #signupForm="ngForm" (ngSubmit)="save(signupForm)">
  <button [disabled]=""!signupForm.valid >
  </button>
</form>
```
- Attributes:
    - dirty => has been changed (text typed, state changed etc.)
    - touched => user... well... TOUCHED an input

- Data binding:
    - 'name' is a FormControl's key
    - #firstNameVar is our variable to access FormControl
    
```
<form>
  <input [(ngModel)]="customer.firstName"
  name="firstName"
  #firstNameVar="ngModel">
</form>
```
Using variables:
```
<span *ngIf="firstNameVar.errors" >Error</span>
```
2. Reactive (model-driven)
    - manage data in component code (ex. validation)
    - easier to change data (upper case, casting etc.)
    - reactive transformations (debounce time, distinct until changed)
    - adding elements dynamically
    - easier to unit test
    - we create our own form model
    - ReactiveFormsModule - formGroup, dormCOntrol, formControlName, formGroupName, formArrayName

Input validation styling:
 ```
 <input [ngClass]="{'is-invalid': formError.firstName}" >
 ```
 ```
 <span *ngIf="formError.firstName" >{{formError.firstName}}</span>
 ```
