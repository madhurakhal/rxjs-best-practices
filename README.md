# rxjs-best-practices

### Creating and Subscribing subjects
1. Don't expose subject so that user can change it. 
`
private subject$ = new Subject();
// exposing as observable
public subjectObservables = this.subject$.asObservable();
`
2. Event Bus
`import { Injectable } from '@angular/core';
import { Subject, Subscription } from 'rxjs';
import { filter, map } from 'rxjs/operators';


@Injectable()
export class EventBusService {
  private subject = new Subject<any>();
  constructor() {}

  on(event: Event, action: any): Subscription {
    return this.subject
      .pipe(
        filter((e: EmitEvent) => {
          return e.name === event;
        }),
        map((e: EmitEvent) => {
          return e.value;
        })
      )
      .subscribe(action);
  }
  emit(event: EmitEvent) {
    this.subject.next(event);
  }
}

export class EmitEvent {
  constructor(public name: any, public value?: any) {}
}

export enum Events {
  CustomerSelected
}
`
