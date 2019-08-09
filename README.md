# rxjs-best-practices

### Creating and Subscribing subjects
1. Don't expose subject so that user can change it. 
`
private subject$ = new Subject();
// exposing as observable
public subjectObservables = this.subject$.asObservable();
`
