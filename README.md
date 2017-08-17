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
