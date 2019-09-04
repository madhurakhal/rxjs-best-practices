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

### Unsubscribing
1. unsubscribe()
2. autounsubscribe package
3. subsink package.


### Different operators.
1. `switchMap` Cancels the current subscription/request and can case race condition `Use for get requests or cancelable requests like searches`
2. `concatMap` Runs subscriptions/requests in order and is less performant. `Use for get, post and put requests when order is important`
3. `mergeMap` Runs subscriptions/requests in parallel. `Use for put, post and delete methods when order is not important`
4. `exhaustMap` Ignores all subsequent subscriptions/requests until it completes. `Use for login when you do not want more requests until the initial one is complete.`
