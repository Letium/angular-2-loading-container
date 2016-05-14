# Loading Indicator

This is meant to be a super-simple Angualr 2 component, used in tandem with `ngSwitch` to block DOM elements while waiting for async data.

## To Use:

Pull `loading-container.ts` into your project, generally in a widely accessible place, where it can be referenced by other classes.  Pull `loading-container.scss` into your Sass directory.  

In your target class, reference the two classes in `loading-container.ts`.  One is a directive, while the other is a base class that your component should extend:

	import {Component, OnInit} from 'angular/core';
	import {DataService} from '../../data/data.service';
	import {LoadingContainer, LoadingPage} from '../../components/loading-container';
	
	@Component({
		selector: 'some-component',
	    templateUrl: 'some/page.html',
	    providers: [DataService],
	    directives: [LoadingContainer]
	})
	export class LoginPage extends LoadingPage {
		public asyncData;
	
	    constructor(private _data: DataService) {
	        super(true); // sets loading to true
	    }
	
	    ngOnInit() {
	        this._data.getData().then(res => {
	            this.asyncData = res;
	            this.ready(); // sets loading to false
	        );
	    }
	}

In the template where you'll use this class, add an `*ngSwitch` to toggle the active view:

	<ion-content class="login">
		<loading-container [loading]="loading">
			<login-form></login-form>
		</loading-container>
	</ion-content>

In this case, the `ngSwitch` blocks the `ngFor` repeater from loading into the DOM; had it been loaded, Angular would have thrown an error, as it wouldn't be able to find property `items` of *undefined*.

### Members

The base class comes with two methods, a public property, and a constructor.

#### `public loading: boolean`

A property that will be set by the various internal methods, and will ultimately set the value of the `ngSwitch` in your component's template.

#### `constructor(value: boolean)`

The class constructor method, which apply an initial parameter value to the `loading` property.

#### `public ready()`

A method that sets `loading` to false, thus activating the part of the template that had been blocked.

#### `public standby()`

A method that sets `loading` to true, thus re-blocking the part of the template that you don't want rendered.