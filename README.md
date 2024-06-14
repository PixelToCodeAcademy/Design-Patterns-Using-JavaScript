# Design-Patterns-Using-JavaScript

### What is a Design Pattern?

A design pattern in software development is a general, reusable solution to a commonly occurring problem within a given context. Design patterns are like templates designed to help you solve problems that can be adapted to fit different situations. They represent best practices used by experienced object-oriented software developers.

### Categories of Design Patterns

Design patterns are typically divided into three main categories:
1. **Creational Patterns**: Deal with object creation mechanisms.
2. **Structural Patterns**: Deal with object composition.
3. **Behavioral Patterns**: Deal with object interaction and responsibility.

### Creational Patterns

1. **Factory Method**: Defines an interface for creating an object but lets subclasses alter the type of objects that will be created.
   - **Example in JavaScript**:
     ```javascript
     class Product {
         constructor(name) {
             this.name = name;
         }
     }

     class ProductFactory {
         createProduct(type) {
             if (type === 'ProductA') {
                 return new Product('ProductA');
             } else if (type === 'ProductB') {
                 return new Product('ProductB');
             }
         }
     }

     const factory = new ProductFactory();
     const productA = factory.createProduct('ProductA');
     console.log(productA.name); // ProductA
     ```

2. **Abstract Factory**: Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
   - **Example in JavaScript**:
     ```javascript
     class Car {
         constructor() {
             this.type = 'Car';
         }
     }

     class Bike {
         constructor() {
             this.type = 'Bike';
         }
     }

     class VehicleFactory {
         createVehicle(vehicleType) {
             if (vehicleType === 'Car') {
                 return new Car();
             } else if (vehicleType === 'Bike') {
                 return new Bike();
             }
         }
     }

     const factory = new VehicleFactory();
     const car = factory.createVehicle('Car');
     console.log(car.type); // Car
     ```

3. **Builder**: Separates the construction of a complex object from its representation so that the same construction process can create different representations.
   - **Example in JavaScript**:
     ```javascript
     class Car {
         constructor() {
             this.make = '';
             this.model = '';
         }
     }

     class CarBuilder {
         constructor() {
             this.car = new Car();
         }

         setMake(make) {
             this.car.make = make;
             return this;
         }

         setModel(model) {
             this.car.model = model;
             return this;
         }

         build() {
             return this.car;
         }
     }

     const carBuilder = new CarBuilder();
     const car = carBuilder.setMake('Toyota').setModel('Camry').build();
     console.log(car); // Car { make: 'Toyota', model: 'Camry' }
     ```

4. **Prototype**: Used to create objects by copying an existing object, known as the prototype.
   - **Example in JavaScript**:
     ```javascript
     const carPrototype = {
         make: 'default',
         model: 'default',
         clone() {
             return Object.assign({}, this);
         }
     };

     const car = carPrototype.clone();
     car.make = 'Honda';
     car.model = 'Civic';
     console.log(car); // { make: 'Honda', model: 'Civic', clone: [Function: clone] }
     ```

5. **Singleton**: Ensures a class has only one instance and provides a global point of access to it.
   - **Example in JavaScript**:
     ```javascript
     class Singleton {
         constructor() {
             if (!Singleton.instance) {
                 Singleton.instance = this;
             }
             return Singleton.instance;
         }
     }

     const instance1 = new Singleton();
     const instance2 = new Singleton();
     console.log(instance1 === instance2); // true
     ```

### Structural Patterns

1. **Adapter**: Allows incompatible interfaces to work together.
   - **Example in JavaScript**:
     ```javascript
     class OldCalculator {
         operations(t1, t2, operation) {
             switch (operation) {
                 case 'add': return t1 + t2;
                 case 'sub': return t1 - t2;
             }
         }
     }

     class NewCalculator {
         add(t1, t2) {
             return t1 + t2;
         }

         sub(t1, t2) {
             return t1 - t2;
         }
     }

     class CalculatorAdapter {
         constructor() {
             this.newCalculator = new NewCalculator();
         }

         operations(t1, t2, operation) {
             switch (operation) {
                 case 'add': return this.newCalculator.add(t1, t2);
                 case 'sub': return this.newCalculator.sub(t1, t2);
             }
         }
     }

     const adapter = new CalculatorAdapter();
     console.log(adapter.operations(10, 5, 'add')); // 15
     ```

2. **Bridge**: Separates an object’s abstraction from its implementation.
   - **Example in JavaScript**:
     ```javascript
     class Abstraction {
         constructor(implementor) {
             this.implementor = implementor;
         }

         operation() {
             return this.implementor.operationImp();
         }
     }

     class ConcreteImplementorA {
         operationImp() {
             return 'ConcreteImplementorA';
         }
     }

     class ConcreteImplementorB {
         operationImp() {
             return 'ConcreteImplementorB';
         }
     }

     const abstractionA = new Abstraction(new ConcreteImplementorA());
     console.log(abstractionA.operation()); // ConcreteImplementorA

     const abstractionB = new Abstraction(new ConcreteImplementorB());
     console.log(abstractionB.operation()); // ConcreteImplementorB
     ```

3. **Composite**: Composes objects into tree structures to represent part-whole hierarchies.
   - **Example in JavaScript**:
     ```javascript
     class Component {
         constructor(name) {
             this.name = name;
         }

         getName() {
             return this.name;
         }
     }

     class Composite extends Component {
         constructor(name) {
             super(name);
             this.children = [];
         }

         add(component) {
             this.children.push(component);
         }

         remove(component) {
             this.children = this.children.filter(child => child !== component);
         }

         getName() {
             return `${super.getName()} with children: ${this.children.map(child => child.getName()).join(', ')}`;
         }
     }

     const tree = new Composite('root');
     const branch1 = new Composite('branch1');
     const leaf1 = new Component('leaf1');

     tree.add(branch1);
     branch1.add(leaf1);

     console.log(tree.getName()); // root with children: branch1 with children: leaf1
     ```

4. **Decorator**: Adds responsibilities to objects dynamically.
   - **Example in JavaScript**:
     ```javascript
     class Coffee {
         cost() {
             return 5;
         }
     }

     class MilkDecorator {
         constructor(coffee) {
             this.coffee = coffee;
         }

         cost() {
             return this.coffee.cost() + 1;
         }
     }

     class SugarDecorator {
         constructor(coffee) {
             this.coffee = coffee;
         }

         cost() {
             return this.coffee.cost() + 0.5;
         }
     }

     let coffee = new Coffee();
     coffee = new MilkDecorator(coffee);
     coffee = new SugarDecorator(coffee);

     console.log(coffee.cost()); // 6.5
     ```

5. **Facade**: Provides a simplified interface to a complex system.
   - **Example in JavaScript**:
     ```javascript
     class CPU {
         freeze() { console.log('CPU freeze'); }
         jump(position) { console.log(`CPU jump to ${position}`); }
         execute() { console.log('CPU execute'); }
     }

     class Memory {
         load(position, data) { console.log(`Memory load ${data} at ${position}`); }
     }

     class HardDrive {
         read(lba, size) { console.log(`HardDrive read ${size} bytes at ${lba}`); }
     }

     class ComputerFacade {
         constructor() {
             this.cpu = new CPU();
             this.memory = new Memory();
             this.hardDrive = new HardDrive();
         }

         start() {
             this.cpu.freeze();
             this.memory.load(0, 'bootloader');
             this.cpu.jump(0);
             this.cpu.execute();
         }
     }

     const computer = new ComputerFacade();
     computer.start(); // Simplified interface to start the computer
     ```

6. **Flyweight**: Reduces the cost of creating and manipulating a large number of similar objects.
   - **Example in JavaScript**:
     ```javascript
     class Flyweight {
         constructor(sharedState) {
             this.sharedState = sharedState;
         }

         operation(uniqueState) {
             console.log(`Flyweight: shared (${this.sharedState}) and unique (${uniqueState}) state.`);
         }
     }

     class FlyweightFactory {
         constructor() {
             this.flyweights = {};
         }

         getFlyweight(sharedState) {
             if (!this.flyweights[sharedState]) {
                 this.flyweights[sharedState] = new Flyweight(sharedState);
             }
             return this.flyweights[sharedState];
         }

         getFlyweightCount() {
             return Object.keys(this.flyweights).length;
         }
     }

     const factory = new FlyweightFactory();
     const flyweight1 = factory.getFlyweight('shared1');
     flyweight1.operation('

unique1');
     const flyweight2 = factory.getFlyweight('shared1');
     flyweight2.operation('unique2');
     console.log(factory.getFlyweightCount()); // 1
     ```

7. **Proxy**: Provides a surrogate or placeholder for another object to control access to it.
   - **Example in JavaScript**:
     ```javascript
     class RealSubject {
         request() {
             console.log('RealSubject request');
         }
     }

     class ProxySubject {
         constructor(realSubject) {
             this.realSubject = realSubject;
         }

         request() {
             console.log('Proxy request');
             this.realSubject.request();
         }
     }

     const realSubject = new RealSubject();
     const proxy = new ProxySubject(realSubject);
     proxy.request(); // Proxy request -> RealSubject request
     ```

### Behavioral Patterns

1. **Chain of Responsibility**: Passes a request along a chain of handlers.
   - **Example in JavaScript**:
     ```javascript
     class Handler {
         setNext(handler) {
             this.nextHandler = handler;
             return handler;
         }

         handle(request) {
             if (this.nextHandler) {
                 return this.nextHandler.handle(request);
             }
             return null;
         }
     }

     class ConcreteHandler1 extends Handler {
         handle(request) {
             if (request === 'request1') {
                 return `Handled by ConcreteHandler1`;
             }
             return super.handle(request);
         }
     }

     class ConcreteHandler2 extends Handler {
         handle(request) {
             if (request === 'request2') {
                 return `Handled by ConcreteHandler2`;
             }
             return super.handle(request);
         }
     }

     const handler1 = new ConcreteHandler1();
     const handler2 = new ConcreteHandler2();

     handler1.setNext(handler2);

     console.log(handler1.handle('request1')); // Handled by ConcreteHandler1
     console.log(handler1.handle('request2')); // Handled by ConcreteHandler2
     ```

2. **Command**: Encapsulates a request as an object, thereby allowing for parameterization of clients with different requests.
   - **Example in JavaScript**:
     ```javascript
     class Command {
         execute() {}
     }

     class Receiver {
         action() {
             console.log('Receiver action');
         }
     }

     class ConcreteCommand extends Command {
         constructor(receiver) {
             super();
             this.receiver = receiver;
         }

         execute() {
             this.receiver.action();
         }
     }

     class Invoker {
         setCommand(command) {
             this.command = command;
         }

         executeCommand() {
             this.command.execute();
         }
     }

     const receiver = new Receiver();
     const command = new ConcreteCommand(receiver);
     const invoker = new Invoker();

     invoker.setCommand(command);
     invoker.executeCommand(); // Receiver action
     ```

3. **Interpreter**: Defines a grammatical representation for a language and an interpreter to interpret the grammar.
   - **Example in JavaScript**:
     ```javascript
     class Context {
         constructor(input) {
             this.input = input;
             this.output = 0;
         }
     }

     class Expression {
         interpret(context) {
             if (context.input.length === 0) {
                 return;
             }

             if (context.input.startsWith('one')) {
                 context.output += 1;
                 context.input = context.input.substring(3);
             }
         }
     }

     const context = new Context('oneone');
     const expression = new Expression();

     while (context.input.length > 0) {
         expression.interpret(context);
     }

     console.log(context.output); // 2
     ```

4. **Iterator**: Provides a way to access elements of a collection sequentially without exposing its underlying representation.
   - **Example in JavaScript**:
     ```javascript
     class Iterator {
         constructor(items) {
             this.index = 0;
             this.items = items;
         }

         next() {
             return this.items[this.index++];
         }

         hasNext() {
             return this.index < this.items.length;
         }
     }

     const items = [1, 2, 3, 4];
     const iterator = new Iterator(items);

     while (iterator.hasNext()) {
         console.log(iterator.next()); // 1, 2, 3, 4
     }
     ```

5. **Mediator**: Defines an object that encapsulates how a set of objects interact.
   - **Example in JavaScript**:
     ```javascript
     class Mediator {
         notify(sender, event) {}
     }

     class ConcreteMediator extends Mediator {
         constructor(component1, component2) {
             super();
             this.component1 = component1;
             this.component1.setMediator(this);
             this.component2 = component2;
             this.component2.setMediator(this);
         }

         notify(sender, event) {
             if (event === 'A') {
                 console.log('Mediator reacts on A and triggers B');
                 this.component2.doC();
             }

             if (event === 'D') {
                 console.log('Mediator reacts on D and triggers C');
                 this.component1.doB();
             }
         }
     }

     class BaseComponent {
         setMediator(mediator) {
             this.mediator = mediator;
         }
     }

     class Component1 extends BaseComponent {
         doA() {
             console.log('Component 1 does A.');
             this.mediator.notify(this, 'A');
         }

         doB() {
             console.log('Component 1 does B.');
         }
     }

     class Component2 extends BaseComponent {
         doC() {
             console.log('Component 2 does C.');
         }

         doD() {
             console.log('Component 2 does D.');
             this.mediator.notify(this, 'D');
         }
     }

     const c1 = new Component1();
     const c2 = new Component2();
     const mediator = new ConcreteMediator(c1, c2);

     c1.doA(); // Component 1 does A. Mediator reacts on A and triggers B. Component 2 does C.
     c2.doD(); // Component 2 does D. Mediator reacts on D and triggers C. Component 1 does B.
     ```

6. **Memento**: Captures and externalizes an object’s internal state so that the object can be restored to this state later.
   - **Example in JavaScript**:
     ```javascript
     class Memento {
         constructor(state) {
             this.state = state;
         }

         getState() {
             return this.state;
         }
     }

     class Originator {
         setState(state) {
             this.state = state;
         }

         getState() {
             return this.state;
         }

         saveStateToMemento() {
             return new Memento(this.state);
         }

         getStateFromMemento(memento) {
             this.state = memento.getState();
         }
     }

     class Caretaker {
         constructor() {
             this.mementoList = [];
         }

         add(memento) {
             this.mementoList.push(memento);
         }

         get(index) {
             return this.mementoList[index];
         }
     }

     const originator = new Originator();
     const caretaker = new Caretaker();

     originator.setState('State #1');
     originator.setState('State #2');
     caretaker.add(originator.saveStateToMemento());

     originator.setState('State #3');
     caretaker.add(originator.saveStateToMemento());

     originator.setState('State #4');

     console.log('Current State: ' + originator.getState()); // Current State: State #4
     originator.getStateFromMemento(caretaker.get(0));
     console.log('First saved State: ' + originator.getState()); // First saved State: State #2
     originator.getStateFromMemento(caretaker.get(1));
     console.log('Second saved State: ' + originator.getState()); // Second saved State: State #3
     ```

7. **Observer**: Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
   - **Example in JavaScript**:
     ```javascript
     class Subject {
         constructor() {
             this.observers = [];
         }

         addObserver(observer) {
             this.observers.push(observer);
         }

         removeObserver(observer) {
             this.observers = this.observers.filter(obs => obs !== observer);
         }

         notifyObservers() {
             this.observers.forEach(observer => observer.update());
         }
     }

     class Observer {
         update() {
             console.log('Observer update');
         }
     }

     const subject = new Subject();
     const observer1 = new Observer();
     const observer2 = new Observer();

     subject.addObserver(observer1);
     subject.addObserver(observer2);

     subject.notifyObservers(); // Observer update, Observer update
     ```

8. **State**: Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.
   - **Example in JavaScript**:
     ```javascript
     class State {
         handle(context) {}
     }

     class ConcreteStateA extends State {
         handle(context) {
             console.log('ConcreteStateA handles request.');
             context.setState(new ConcreteStateB());
         }
     }

     class ConcreteStateB extends State {
         handle(context) {
             console.log('ConcreteStateB handles request.');
             context.setState(new ConcreteStateA());
         }
     }

     class Context {
         constructor(state) {
             this.setState(state);
         }

         setState(state) {
             this.state = state;
         }

         request() {
             this.state.handle(this);
         }
     }

     const

 context = new Context(new ConcreteStateA());
     context.request(); // ConcreteStateA handles request.
     context.request(); // ConcreteStateB handles request.
     ```

9. **Strategy**: Defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.
   - **Example in JavaScript**:
     ```javascript
     class Strategy {
         execute(a, b) {}
     }

     class ConcreteStrategyAdd extends Strategy {
         execute(a, b) {
             return a + b;
         }
     }

     class ConcreteStrategySubtract extends Strategy {
         execute(a, b) {
             return a - b;
         }
     }

     class ConcreteStrategyMultiply extends Strategy {
         execute(a, b) {
             return a * b;
         }
     }

     class Context {
         setStrategy(strategy) {
             this.strategy = strategy;
         }

         executeStrategy(a, b) {
             return this.strategy.execute(a, b);
         }
     }

     const context = new Context();
     context.setStrategy(new ConcreteStrategyAdd());
     console.log(context.executeStrategy(3, 4)); // 7

     context.setStrategy(new ConcreteStrategySubtract());
     console.log(context.executeStrategy(10, 5)); // 5

     context.setStrategy(new ConcreteStrategyMultiply());
     console.log(context.executeStrategy(6, 7)); // 42
     ```

10. **Template Method**: Defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm’s structure.
    - **Example in JavaScript**:
      ```javascript
      class AbstractClass {
          templateMethod() {
              this.step1();
              this.step2();
              this.step3();
          }

          step1() {
              console.log('AbstractClass step1');
          }

          step2() {
              throw new Error('Abstract method!');
          }

          step3() {
              console.log('AbstractClass step3');
          }
      }

      class ConcreteClass extends AbstractClass {
          step2() {
              console.log('ConcreteClass step2');
          }
      }

      const concrete = new ConcreteClass();
      concrete.templateMethod(); // AbstractClass step1, ConcreteClass step2, AbstractClass step3
      ```

11. **Visitor**: Represents an operation to be performed on elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.
    - **Example in JavaScript**:
      ```javascript
      class Visitor {
          visitConcreteElementA(element) {}
          visitConcreteElementB(element) {}
      }

      class ConcreteVisitor1 extends Visitor {
          visitConcreteElementA(element) {
              console.log('ConcreteVisitor1 visiting', element.getName());
          }

          visitConcreteElementB(element) {
              console.log('ConcreteVisitor1 visiting', element.getName());
          }
      }

      class Element {
          accept(visitor) {}
      }

      class ConcreteElementA extends Element {
          accept(visitor) {
              visitor.visitConcreteElementA(this);
          }

          getName() {
              return 'ConcreteElementA';
          }
      }

      class ConcreteElementB extends Element {
          accept(visitor) {
              visitor.visitConcreteElementB(this);
          }

          getName() {
              return 'ConcreteElementB';
          }
      }

      const elements = [new ConcreteElementA(), new ConcreteElementB()];
      const visitor = new ConcreteVisitor1();

      elements.forEach(element => element.accept(visitor));
      // ConcreteVisitor1 visiting ConcreteElementA
      // ConcreteVisitor1 visiting ConcreteElementB
      ```

These examples provide a basic understanding of design patterns in JavaScript. For a deeper dive, consider studying more comprehensive examples and real-world use cases.
