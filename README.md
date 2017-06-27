# Angular2

* [Fine grained change detection with Angular 2](https://juristr.com/blog/2016/04/angular2-change-detection/)

### Service Workers
* [Fast Offline Angular Apps with Service Workers](https://coryrylan.com/blog/fast-offline-angular-apps-with-service-workers)
* [The offline cookbook](https://jakearchibald.com/2014/offline-cookbook/)

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
  <div class='project-class'>
    ProjectClass
  </div>

  <div>
    ProjectedElement
  </div>

  <div projectAttr>
    ProjectAttr
  </div>
 ``` 
 
 ```html
  <ng-content select=".project-class"> </ng-content>
  <ng-content select="[projectAttr]"> </ng-content>
  <ng-content select="div"> </ng-content>
```
