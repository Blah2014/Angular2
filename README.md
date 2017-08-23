# Angular2

* [Fine grained change detection with Angular 2](https://juristr.com/blog/2016/04/angular2-change-detection/)

### Service Workers
* [Fast Offline Angular Apps with Service Workers](https://coryrylan.com/blog/fast-offline-angular-apps-with-service-workers)
* [The offline cookbook](https://jakearchibald.com/2014/offline-cookbook/)

### Angular Mobile Toolkit
* [Automatic Progressive Web Apps](https://mobile.angular.io)

### Angular CLI
* [A command line interface for Angular](https://cli.angular.io)

### ```<ng-container>```
The Angular ```<ng-container>``` is a grouping element that doesn't interfere with styles or layout because Angular doesn't put it in the DOM.
```html
<p>
  I turned the corner
  <ng-container *ngIf="hero">
    and saw {{hero.name}}. I waved
  </ng-container>
  and continued on my way.
</p>
```

### ```<ng-content>```
The ```<ng-content>``` tag is a placeholder for the external content. It tells Angular where to insert that content.
```html
// Using the component
<foo-bar>
  <div class='project-class'>
    ProjectClass
  </div>

  <div>
    ProjectedElement
  </div>

  <div projectAttr>
    ProjectAttr
  </div>
 </foo-bar>
 ``` 
 
 ```html
 // Component template
<ng-content select=".project-class"> </ng-content>
<ng-content select="[projectAttr]"> </ng-content>
<ng-content select="div"> </ng-content>
```

### Angular2 - catch/subscribe to (click) event in dynamically added HTML  
Declarative event binding is only supported in static HTML in a components template.
If you want to subscribe to events of elements dynamically added, you need to do it imperatively.
```
elementRef.nativeElement.querySelector(...).addEventListener(...)
```
or similar.

If you want to be WebWorker-safe, you can inject the Renderer
```
constructor(private elementRef:ElementRef, private renderer:Renderer) {}
```
and use instead
```
this.renderer.listen(this.elementRef.nativeElement, 'click', (event) => { handleClick(event);});
```
to register an event handler.

You can use ```listenGlobal``` that will give you access to ```document```, ```body```, etc.
```
this.renderer.listenGlobal('document', 'click', (event) => {
  // Do something with 'event'
});
```

Note that since ```beta.2``` both ```listen``` and ```listenGlobal``` return a function to remove the listener. This is to avoid memory leaks in big applications.
```
// listenFunc will hold the function returned by "renderer.listen"
listenFunc: Function;

// globalListenFunc will hold the function returned by "renderer.listenGlobal"
globalListenFunc: Function;

constructor(elementRef: ElementRef, renderer: Renderer) {

    // We cache the function "listen" returns
    this.listenFunc = renderer.listen(elementRef.nativeElement, 'click', (event) => {
        // Do something with 'event'
    });

    // We cache the function "listenGlobal" returns
    this.globalListenFunc = renderer.listenGlobal('document', 'click', (event) => {
        // Do something with 'event'
    });
}

ngOnDestroy() {
    // We execute both functions to remove the respectives listeners

    // Removes "listen" listener
    this.listenFunc();

    // Removs "listenGlobal" listener
    this.globalListenFunc();
}
```

### takeWhile()

The TakeWhile mirrors the source Observable until such time as some condition you specify becomes false, at which point TakeWhile stops mirroring the source Observable and terminates its own Observable.

First, we need to import the ```takeWhile()``` operator:
```
import "rxjs/add/operator/takeWhile";
```

Then, let’s use the ```takeWhile()``` operator with our observable that is returned from ```UserService.authenticate()```:
```
export class MyComponent implements OnDestroy, OnInit {

  public user: User;
  private alive: boolean = true;

  public ngOnInit() {
    this.userService.authenticate(email, password)
      .takeWhile(() => this.alive)
      .subscribe(user => {
        this.user = user;
      });
  }

  public ngOnDestroy() {
    this.alive = false;
  }

}
```

First, I defined a private boolean variable named ```alive``` that is set to true. Next, we provide a function to the ```takeWhile()``` operator that returns the boolean value (that is initially true). Finally, we set the value of ```alive``` to ```false``` when the component is destroyed.

As you can see, the ```takeWhile()``` operator is an excellent solution to unsubscribing from an observable subscription as part of Angular’s component lifecycle.

### Observable
```
import { Observable } from 'rxjs/Observable'

test(val): Observable<any> {
    return new Observable((observable) => {
        if(val) {
            observable.next('success');
        } else {
            observable.error('error');
        }
    });
}

this.test(false)
    .subscribe(
        (success) => {
            console.log(success);
        },
        (error) => {
            console.log(error);
        }
    );
```

Don't forget to ```unsubscribe()``` or ```takeWhile()```

### Promise
```
test(val): Promise<any> {
      return new Promise((resolve, reject) => {
          if(val) {
              resolve('success');
          } else {
              reject('error');
          }
      });
  }
  
this.test(false)
    .then(
        (success) => {
            console.log(success);
        },
        (error) => {
            console.log(error);
        }
    );
```
