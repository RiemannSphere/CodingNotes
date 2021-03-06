**Angular Reactive Forms by Deborah Kurata - Pluralsight**  
**Example code with routing, CRUD etc.:**  
**https://github.com/DeborahK/Angular-ReactiveForms**  
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
```
save(cutsomerForm: NgForm){
    ...
}
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
 
Basic form:
```
<-- Here we bind a property -->
<form [formGroup]="customerForm" >
```
```
<-- Here we bind to a simple string name -->
<input formControlName="firstName" >
```
Accessing form model properties, two simple ways:
```
customerForm.controls.firstName.valid
customerForm.get('firstName').valid
```
Another way, separate property:
```
firstName = new FormControl();
ngOnInit() {
    this.customerForm = new FOrmGroup({
        firstName: this.firstName
    });
}
...
firstName.valid
```
- **Remember**
    1. delete ngModel directive (use formControlName instead)
    2. delette name attribute
    3. delete template reference varibales (use customerForm.get('x') instead)
- setValue and patchValue
    - setValue sets entire form, forGroup or formControl value
    - patchValue allows you to update only certain values of form or formGroup
- FormBuilder:
    - Import FormBuilder -> inject FormBuilder -> use the instance.
    - Declaring values is easier, we don't have to instantiate formControls and formGroups.
    - We can use arrays.
    - We can use object to disable/enable element.
    ```
    this.customerForm = this.fb.group({
        firstName: '',
        lastName: {value: '', disabled: true},
        address: [''],
        email: [{value: '', disabled: true}]
        ...
    });
    ```
    - Validation:
    ```
    firstName: ['', Validators.require]
    lastName: ['', [Validators.require, Validators.minlength(3), ...]]
    email: [default, sync validators, async validators]
    ```
- Change validation:
```
myControl.setValidators(...);
myControl.clearValidators();
// after changing validators we invoke:
myControl.updateValueAndValidity();
```
- Custom Validator:
```
function myValidator(c: AbstractControl): {[key: string]: boolean} | null {
    if(sthIsWrong) {
        return {'myvalidator': true};
    }
    return null; //null means control is valid
}
```
for example:
```
function ratingRange(c: AbstractControl): {[key: string]: boolean} | null {
    if( c.value !== null && (isNan(c.value) || c.value < 1 || c.value > 5) ){
        return {'range': true};
    }
    return null;
}
// we use it inside our form
this.customerForm = this.fb.group({
    rating: [null, ratingRange]
});
```
- Validator with a parameter -> validator that returns a validator (**factory function**, functional wrapper for our regular validator):
```
function ratingRange(min: number, max: number): ValidatorFn {
    return (c: AbstractControl): {[key: string]: boolean} | null => {
        if( c.value !== null && (isNan(c.value) || c.value < min || c.value > max) ){
            return {'myvalidator': true};
        }
        return null;
    }
}
// usage
rating: [null, ratingRange(1,5)]
```
- Cross-field validation (ex. email and email confirmation):
```
availability: this.fb.group({
    start: ['', Validators.require],
    end: ['', Validators.require]
}, {validator: myValidator})
```
```
<div formGroupName="availability" >
    <input formControlName="start" >
    <input formControlName="end" >
</div>
```
- Watching changes (in control, group or form). Returns an Obserbavle<any>:
```
myControl.valueChanges.subscribe( value => {...} );
```
- Reacting to changes:
    - Change elements
    - Send message
    - **Input suggestion**
- Reactive transformations (**rxjs**)
    - debounce time (ignores events for given period of time)
    - throttleTime
    - distinctUntilChanged
- FormArray - array of FormGroups or FormControls indexed by integers 0...n.
    ```
    array: this.gb.array([this.createElement()])
    ```
    HTML:
    ```
    <div formArrayName="addresses" *ngFor=""let address of addresses.controls; let i=index >
        <div [formGroupName]="i" >
            <label attr.for="{{'street' + i}}" >
            <input id="{{'street' + i}}" >
        </div>
        <-- Use property binding as we use variable i. -->
        <-- Use attr.for instead of for -->
        <-- We index id and label to associate them with elements of array. -->
    </div>
    ```
FormArray getter:
    ```
    getAddresses: FormArray {
        return <FormArray>this.customerForm.get('addresses');
    }
    ```
Adding new elements:
    ```
    addAddress(): void{
        this.addresses.push(this.createElement());
    }
    ```
Use event binding:
    ```
    (click)="addAddress()"
    ```
