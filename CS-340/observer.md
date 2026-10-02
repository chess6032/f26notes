# Observer (Design Pattern)

**PROBLEM**: A bunch of objects (observers) need to be notified when some object (subject) changes its state.

## SOLUTION

### Class diagram

![observer pattern class diagram](./observer-solution-class.svg)

### Sequence diagram

Here's what that pattern would look like between a single concrete subject and two concrete observers.

![observer pattern sequence diagram](./observer-solution-sequence.svg)

<!-- 
sequencediagram.org

participant App
participant Subject
participant Observer A
participant Observer B

activate App

  App->Subject: Attach(ObseverA)
  activate Subject
    space
  deactivate Subject

  App->Subject: Attach(ObserverB)
  activate Subject
    space 
  deactivate Subject

deactivate App

space 

App->Subject:SetState(newState)
activate Subject
  Subject->Subject: Notify()
  activate Subject

    Subject->Observer A: Update()
    activate Observer A
      Observer A->Subject: GetState()
      activate Subject
      Subject-\->Observer A:state
      deactivate Subject
      space
    deactivate Observer A

    space

    Subject->Observer B: Update()
    activate Observer B
      Observer B->Subject: GetState()
      activate Subject
      Subject-\->Observer B:state
      deactivate Subject
      space
    deactivate Observer B

  deactivate Subject
  space
deactivate Subject
 -->

## CONSEQUENCES

### Pros

- You can flexibily add/remove observers to a subject.
- Subject is not tightly coupled to its Observers.
  - ("Dependency inversion").
- Supports "broadcast communication".

### Cons

- Changing the state of a subject can be far more expensive than you expect.