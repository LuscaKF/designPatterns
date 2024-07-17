TypeScript - Design Patterns - Padrões de Projeto - GOF

## Descrição
Este repositório contém exemplos e explicações sobre os padrões de projeto (Design Patterns) da Gang of Four (GOF), implementados em TypeScript. O objetivo é fornecer uma compreensão prática de como esses padrões podem ser aplicados em projetos de software, utilizando TypeScript como a linguagem de implementação.
Esse foi um projeto desenvolvido totalmente com base no curso de Javascript e TypeScript - front-end e back-end (Full Stack) - Node, Express, noSQL, React, hooks, Redux, Design Patterns

## Índice
1. [Padrões Criacionais](#padrões-criacionais)
   - [Factory Method](#factory-method)
   - [Abstract Factory](#abstract-factory)
   - [Singleton](#singleton)
   - [Builder](#builder)
   - [Prototype](#prototype)
2. [Padrões Estruturais](#padrões-estruturais)
   - [Adapter](#adapter)
   - [Bridge](#bridge)
   - [Composite](#composite)
   - [Decorator](#decorator)
   - [Facade](#facade)
   - [Flyweight](#flyweight)
   - [Proxy](#proxy)
3. [Padrões Comportamentais](#padrões-comportamentais)
   - [Chain of Responsibility](#chain-of-responsibility)
   - [Command](#command)
   - [Interpreter](#interpreter)
   - [Iterator](#iterator)
   - [Mediator](#mediator)
   - [Memento](#memento)
   - [Observer](#observer)
   - [State](#state)
   - [Strategy](#strategy)
   - [Template Method](#template-method)
   - [Visitor](#visitor)

## Padrões Criacionais:

### Factory Method:
```typescript
// Product Interface
interface Product {
    operation(): string;
}

// Concrete Products
class ConcreteProductA implements Product {
    public operation(): string {
        return 'Result of ConcreteProductA';
    }
}

class ConcreteProductB implements Product {
    public operation(): string {
        return 'Result of ConcreteProductB';
    }
}

// Creator
abstract class Creator {
    public abstract factoryMethod(): Product;

    public someOperation(): string {
        const product = this.factoryMethod();
        return `Creator: The same creator's code has just worked with ${product.operation()}`;
    }
}

// Concrete Creators
class ConcreteCreatorA extends Creator {
    public factoryMethod(): Product {
        return new ConcreteProductA();
    }
}

class ConcreteCreatorB extends Creator {
    public factoryMethod(): Product {
        return new ConcreteProductB();
    }
}

// Client code
function clientCode(creator: Creator) {
    console.log(creator.someOperation());
}

clientCode(new ConcreteCreatorA());
clientCode(new ConcreteCreatorB());
```

### Abstract Factory:
```typescript
// Abstract Products
interface AbstractProductA {
    usefulFunctionA(): string;
}

interface AbstractProductB {
    usefulFunctionB(): string;
    anotherUsefulFunctionB(collaborator: AbstractProductA): string;
}

// Concrete Products
class ConcreteProductA1 implements AbstractProductA {
    public usefulFunctionA(): string {
        return 'The result of the product A1.';
    }
}

class ConcreteProductA2 implements AbstractProductA {
    public usefulFunctionA(): string {
        return 'The result of the product A2.';
    }
}

class ConcreteProductB1 implements AbstractProductB {
    public usefulFunctionB(): string {
        return 'The result of the product B1.';
    }

    public anotherUsefulFunctionB(collaborator: AbstractProductA): string {
        const result = collaborator.usefulFunctionA();
        return `The result of the B1 collaborating with (${result})`;
    }
}

class ConcreteProductB2 implements AbstractProductB {
    public usefulFunctionB(): string {
        return 'The result of the product B2.';
    }

    public anotherUsefulFunctionB(collaborator: AbstractProductA): string {
        const result = collaborator.usefulFunctionA();
        return `The result of the B2 collaborating with (${result})`;
    }
}

// Abstract Factory
interface AbstractFactory {
    createProductA(): AbstractProductA;
    createProductB(): AbstractProductB;
}

// Concrete Factories
class ConcreteFactory1 implements AbstractFactory {
    public createProductA(): AbstractProductA {
        return new ConcreteProductA1();
    }

    public createProductB(): AbstractProductB {
        return new ConcreteProductB1();
    }
}

class ConcreteFactory2 implements AbstractFactory {
    public createProductA(): AbstractProductA {
        return new ConcreteProductA2();
    }

    public createProductB(): AbstractProductB {
        return new ConcreteProductB2();
    }
}

// Client code
function clientCode(factory: AbstractFactory) {
    const productA = factory.createProductA();
    const productB = factory.createProductB();

    console.log(productB.usefulFunctionB());
    console.log(productB.anotherUsefulFunctionB(productA));
}

clientCode(new ConcreteFactory1());
clientCode(new ConcreteFactory2());
```

### Singleton:
```typescript
class Singleton {
    private static instance: Singleton;

    private constructor() {}

    public static getInstance(): Singleton {
        if (!Singleton.instance) {
            Singleton.instance = new Singleton();
        }
        return Singleton.instance;
    }

    public someBusinessLogic() {
        // ...
    }
}

// Client code
const s1 = Singleton.getInstance();
const s2 = Singleton.getInstance();

if (s1 === s2) {
    console.log('Singleton works, both variables contain the same instance.');
} else {
    console.log('Singleton failed, variables contain different instances.');
}
```

### Builder:
```typescript
// Product
class Product {
    public parts: string[] = [];

    public listParts(): void {
        console.log(`Product parts: ${this.parts.join(', ')}`);
    }
}

// Builder Interface
interface Builder {
    producePartA(): void;
    producePartB(): void;
    producePartC(): void;
}

// Concrete Builder
class ConcreteBuilder implements Builder {
    private product: Product;

    constructor() {
        this.reset();
    }

    public reset(): void {
        this.product = new Product();
    }

    public producePartA(): void {
        this.product.parts.push('PartA1');
    }

    public producePartB(): void {
        this.product.parts.push('PartB1');
    }

    public producePartC(): void {
        this.product.parts.push('PartC1');
    }

    public getProduct(): Product {
        const result = this.product;
        this.reset();
        return result;
    }
}

// Director
class Director {
    private builder: Builder;

    public setBuilder(builder: Builder): void {
        this.builder = builder;
    }

    public buildMinimalViableProduct(): void {
        this.builder.producePartA();
    }

    public buildFullFeaturedProduct(): void {
        this.builder.producePartA();
        this.builder.producePartB();
        this.builder.producePartC();
    }
}

// Client code
const director = new Director();
const builder = new ConcreteBuilder();
director.setBuilder(builder);

console.log('Standard basic product:');
director.buildMinimalViableProduct();
builder.getProduct().listParts();

console.log('Standard full featured product:');
director.buildFullFeaturedProduct();
builder.getProduct().listParts();

console.log('Custom product:');
builder.producePartA();
builder.producePartC();
builder.getProduct().listParts();
```

### Prototype:
```typescript
interface Prototype {
    clone(): Prototype;
}

// Concrete Prototype
class ConcretePrototype1 implements Prototype {
    public primitive: any;
    public component: object;
    public circularReference: ComponentWithBackReference;

    constructor() {
        this.component = {};
        this.circularReference = new ComponentWithBackReference(this);
    }

    public clone(): this {
        const clone = Object.create(this);

        clone.component = Object.create(this.component);
        clone.circularReference = {
            ...this.circularReference,
            prototype: { ...this },
        };

        return clone;
    }
}

class ComponentWithBackReference {
    public prototype;

    constructor(prototype: Prototype) {
        this.prototype = prototype;
    }
}

// Client code
const p1 = new ConcretePrototype1();
p1.primitive = 245;
p1.component = new Date();
p1.circularReference = new ComponentWithBackReference(p1);

const p2 = p1.clone();

if (p1.primitive === p2.primitive) {
    console.log('Primitive field values have been carried over to a clone.');
} else {
    console.log('Primitive field values have not been copied.');
}

if (p1.component === p2.component) {
    console.log('Component field values have been copied.');
} else {
    console.log('Component field values have been cloned.');
}

if (p1.circularReference === p2.circularReference) {
    console.log('Component with back reference has been cloned.');
} else {
    console.log('Component with back reference has not been copied.');
}

if (p1.circularReference.prototype === p2.circularReference.prototype) {
    console.log('Component with back reference is linked to the prototype.');
} else {
    console.log('Component with back reference is linked to a clone of the prototype.');
}
```

## Padrões Estruturais:

### Adapter:
```typescript
// Target
class Target {
    public request(): string {
        return 'Target: The default target\'s behavior.';
    }
}

// Adaptee
class Adaptee {
    public specificRequest(): string {
        return '.eetpadA eht fo roivaheb laicepS';
    }
}

// Adapter
class Adapter extends Target {
    private adaptee: Adaptee;

    constructor(adaptee: Adaptee) {
        super();
        this.adaptee = adaptee;
    }

    public request(): string {
        const result = this.adaptee.specificRequest().split('').reverse().join('');
        return `Adapter: (TRANSLATED) ${result}`;
    }
}

// Client code
const target = new Target();
console.log(target.request());

const adaptee = new Adaptee();
console.log(`Adaptee: ${adaptee.specificRequest()}`);

const adapter = new Adapter(adaptee);
console.log(adapter.request());
```

### Bridge:
```typescript
// Implementation Interface
interface Implementation {
    operationImplementation(): string;
}

// Concrete Implementations
class ConcreteImplementationA implements Implementation {
    public operationImplementation(): string {
        return 'ConcreteImplementationA: Here\'s the result on the platform A.';
    }
}

class ConcreteImplementationB implements Implementation {
    public operationImplementation(): string {
        return 'ConcreteImplementationB: Here\'s the result on the platform B.';
    }
}

// Abstraction
class Abstraction {
    protected implementation: Implementation;

    constructor(implementation: Implementation) {
        this.implementation = implementation;
    }

    public operation(): string {
        const result = this.implementation.operationImplementation();
        return `Abstraction: Base operation with:\n${result}`;
    }
}

// Extended Abstraction
class ExtendedAbstraction extends Abstraction {
    public operation(): string {
        const result = this.implementation.operationImplementation();
        return `ExtendedAbstraction: Extended operation with:\n${result}`;
    }
}

// Client code
let implementation = new ConcreteImplementationA();
let abstraction = new Abstraction(implementation);
console.log(abstraction.operation());

implementation = new ConcreteImplementationB();
abstraction = new ExtendedAbstraction(implementation);
console.log(abstraction.operation());
```

### Composite:
```typescript
// Component Interface
abstract class Component {
    protected parent: Component | null = null;

    public setParent(parent: Component | null) {
        this.parent = parent;
    }

    public getParent(): Component | null {
        return this.parent;
    }

    public add(component: Component): void {}
    public remove(component: Component): void {}

    public isComposite(): boolean {
        return false;
    }

    public abstract operation(): string;
}

// Leaf
class Leaf extends Component {
    public operation(): string {
        return 'Leaf';
    }
}

// Composite
class Composite extends Component {
    protected children: Component[] = [];

    public add(component: Component): void {
        this.children.push(component);
        component.setParent(this);
    }

    public remove(component: Component): void {
        const componentIndex = this.children.indexOf(component);
        this.children.splice(componentIndex, 1);
        component.setParent(null);
    }

    public isComposite(): boolean {
        return true;
    }

    public operation(): string {
        const results = [];
        for (const child of this.children) {
            results.push(child.operation());
        }

        return `Branch(${results.join('+')})`;
    }
}

// Client code
const simple = new Leaf();
console.log(`Client: I've got a simple component:\n${simple.operation()}`);

const tree = new Composite();
const branch1 = new Composite();
branch1.add(new Leaf());
branch1.add(new Leaf());

const branch2 = new Composite();
branch2.add(new Leaf());

tree.add(branch1);
tree.add(branch2);

console.log(`Client: Now I've got a composite tree:\n${tree.operation()}`);

function clientCode(component: Component) {
    console.log(`RESULT: ${component.operation()}`);
}

clientCode(tree);
clientCode(simple);
```

### Decorator:
```typescript
// Component Interface
interface Component {
    operation(): string;
}

// Concrete Component
class ConcreteComponent implements Component {
    public operation(): string {
        return 'ConcreteComponent';
    }
}

// Decorator
class Decorator implements Component {
    protected component: Component;

    constructor(component: Component) {
        this.component = component;
    }

    public operation(): string {
        return this.component.operation();
    }
}

// Concrete Decorators
class ConcreteDecoratorA extends Decorator {
    public operation(): string {
        return `ConcreteDecoratorA(${super.operation()})`;
    }
}

class ConcreteDecoratorB extends Decorator {
    public operation(): string {
        return `ConcreteDecoratorB(${super.operation()})`;
    }
}

// Client code
function clientCode(component: Component) {
    console.log(`RESULT: ${component.operation()}`);
}

const simple = new ConcreteComponent();
console.log('Client: I\'ve got a simple component:');
clientCode(simple);

const decorator1 = new ConcreteDecoratorA(simple);
const decorator2 = new ConcreteDecoratorB(decorator1);
console.log('Client: Now I\'ve got a decorated component:');
clientCode(decorator2);
```

### Facade:
```typescript
// Subsystem classes
class Subsystem1 {
    public operation1(): string {
        return 'Subsystem1: Ready!';
    }

    public operationN(): string {
        return 'Subsystem1: Go!';
    }
}

class Subsystem2 {
    public operation1(): string {
        return 'Subsystem2: Get ready!';
    }

    public operationZ(): string {
        return 'Subsystem2: Fire!';
    }
}

// Facade
class Facade {
    protected subsystem1: Subsystem1;
    protected subsystem2: Subsystem2;

    constructor(subsystem1: Subsystem1, subsystem2: Subsystem2) {
        this.subsystem1 = subsystem1;
        this.subsystem2 = subsystem2;
    }

    public operation(): string {
        let result = 'Facade initializes subsystems:\n';
        result += this.subsystem1.operation1();
        result += '\n';
        result += this.subsystem2.operation1();
        result += '\n';
        result += 'Facade orders subsystems to perform the action:\n';
        result += this.subsystem1.operationN();
        result += '\n';
        result += this.subsystem2.operationZ();
        return result;
    }
}

// Client code
const subsystem1 = new Subsystem1();
const subsystem2 = new Subsystem2();
const facade = new Facade(subsystem1, subsystem2);
console.log(facade.operation());
```

### Flyweight:
```typescript
// Flyweight
class Flyweight {
    private sharedState: any;

    constructor(sharedState: any) {
        this.sharedState = sharedState;
    }

    public operation(uniqueState: any): void {
        const s = JSON.stringify(this.sharedState);
        const u = JSON.stringify(uniqueState);
        console.log(`Flyweight: Displaying shared (${s}) and unique (${u}) state.`);
    }
}

// Flyweight Factory
class FlyweightFactory {
    private flyweights: { [key: string]: Flyweight } = <any>{};

    constructor(initialFlyweights: string[][]) {
        for (const state of initialFlyweights) {
            this.flyweights[this.getKey(state)] = new Flyweight(state);
        }
    }

    private getKey(state: any[]): string {
        return state.join('_');
    }

    public getFlyweight(sharedState: any[]): Flyweight {
        const key = this.getKey(sharedState);

        if (!(key in this.flyweights)) {
            console.log('FlyweightFactory: Can\'t find a flyweight, creating new one.');
            this.flyweights[key] = new Flyweight(sharedState);
        } else {
            console.log('FlyweightFactory: Reusing existing flyweight.');
        }

        return this.flyweights[key];
    }

    public listFlyweights(): void {
        const count = Object.keys(this.flyweights).length;
        console.log(`\nFlyweightFactory: I have ${count} flyweights:`);
        for (const key in this.flyweights) {
            console.log(key);
        }
    }
}

// Client code
const factory = new FlyweightFactory([
    ['Chevrolet', 'Camaro2018', 'pink'],
    ['Mercedes Benz', 'C300', 'black'],
    ['Mercedes Benz', 'C500', 'red'],
    ['BMW', 'M5', 'red'],
    ['BMW', 'X6', 'white'],
]);

factory.listFlyweights();

function addCarToPoliceDatabase(
    ff: FlyweightFactory, plates: string, owner: string,
    brand: string, model: string, color: string) {
    console.log('\nClient: Adding a car to database.');
    const flyweight = ff.getFlyweight([brand, model, color]);
    flyweight.operation([plates, owner]);
}

addCarToPoliceDatabase(factory, 'CL234IR', 'James Doe', 'BMW', 'M5', 'red');
addCarToPoliceDatabase(factory, 'CL234IR', 'James Doe', 'BMW', 'X1', 'red');

factory.listFlyweights();
```

### Proxy:
```typescript
// Subject Interface
interface Subject {
    request(): void;
}

// Real Subject
class RealSubject implements Subject {
    public request(): void {
        console.log('RealSubject: Handling request.');
    }
}

// Proxy
class ProxySubject implements Subject {
    private realSubject: RealSubject;

    constructor(realSubject: RealSubject) {
        this.realSubject = realSubject;
    }

    public request(): void {
        if (this.checkAccess()) {
            this.realSubject.request();
            this.logAccess();
        }
    }

    private checkAccess(): boolean {
        console.log('Proxy: Checking access prior to firing a real request.');
        return true;
    }

    private logAccess(): void {
        console.log('Proxy: Logging the time of request.');
    }
}

// Client code
function clientCode(subject: Subject) {
    subject.request();
}

console.log('Client: Executing the client code with a real subject:');
const realSubject = new RealSubject();
clientCode(realSubject);

console.log('');

console.log('Client: Executing the same client code with a proxy:');
const proxy = new ProxySubject(realSubject);
clientCode(proxy);
```

## Padrões Comportamentais:

### Chain of Responsibility:
```typescript
// Handler Interface
interface Handler {
    setNext(handler: Handler): Handler;
    handle(request: string): string;
}

// Abstract Handler
abstract class AbstractHandler implements Handler {
    private nextHandler: Handler;

    public setNext(handler: Handler): Handler {
        this.nextHandler = handler;
        return handler;
    }

    public handle(request: string): string {
        if (this.nextHandler) {
            return this.nextHandler.handle(request);
        }

        return null;
    }
}

// Concrete Handlers
class MonkeyHandler extends AbstractHandler {
    public handle(request: string): string {
        if (request === 'Banana') {
            return `Monkey: I'll eat the ${request}.`;
        }
        return super.handle(request);
    }
}

class SquirrelHandler extends AbstractHandler {
    public handle(request: string): string {
        if (request === 'Nut') {
            return `Squirrel: I'll eat the ${request}.`;
        }
        return super.handle(request);
    }
}

class DogHandler extends AbstractHandler {
    public handle(request: string): string {
        if (request === 'MeatBall') {
            return `Dog: I'll eat the ${request}.`;
        }
        return super.handle(request);
    }
}

// Client code
const monkey = new MonkeyHandler();
const squirrel = new SquirrelHandler();
const dog = new DogHandler();

monkey.setNext(squirrel).setNext(dog);

function clientCode(handler: Handler) {
    const foods = ['Nut', 'Banana', 'Cup of coffee'];

    for (const food of foods) {
        console.log(`Client: Who wants a ${food}?`);

        const result = handler.handle(food);
        if (result) {
            console.log(`  ${result}`);
        } else {
            console.log(`  ${food} was left untouched.`);
        }
    }
}

clientCode(monkey);
```

### Command:
```typescript
// Command Interface
interface Command {
    execute(): void;
}

// Simple Command
class SimpleCommand implements Command {
    private payload: string;

    constructor(payload: string) {
        this.payload = payload;
    }

    public execute(): void {
        console.log(`SimpleCommand: See, I can do simple things like printing (${this.payload})`);
    }
}

// Complex Command
class ComplexCommand implements Command {
    private receiver: Receiver;

    private a: string;

    private b: string;

    constructor(receiver: Receiver, a: string, b: string) {
        this.receiver = receiver;
        this.a = a;
        this.b = b;
    }

    public execute(): void {
        console.log('ComplexCommand: Complex stuff should be done by a receiver object.');
        this.receiver.doSomething(this.a);
        this.receiver.doSomethingElse(this.b);
    }
}

// Receiver
class Receiver {
    public doSomething(a: string): void {
        console.log(`Receiver: Working on (${a}.)`);
    }

    public doSomethingElse(b: string): void {
        console.log(`Receiver: Also working on (${b}.)`);
    }
}

// Invoker
class Invoker {
    private onStart: Command;

    private onFinish: Command;

    public setOnStart(command: Command): void {
        this.onStart = command;
    }

    public setOnFinish(command: Command): void {
        this.onFinish = command;
    }

    public doSomethingImportant(): void {
        console.log('Invoker: Does anybody want something done before I begin?');
        if (this.onStart) {
            this.onStart.execute();
        }

        console.log('Invoker: ...doing something really important...');

        console.log('Invoker: Does anybody want something done after I finish?');
        if (this.onFinish) {
            this.onFinish.execute();
        }
    }
}

// Client code
const invoker = new Invoker();
invoker.setOnStart(new SimpleCommand('Say Hi!'));
const receiver = new Receiver();
invoker.setOnFinish(new ComplexCommand(receiver, 'Send email', 'Save report'));

invoker.doSomethingImportant();
```

### Interpreter:
```typescript
// Context
class Context {
    private data: Map<string, boolean>;

    constructor() {
        this.data = new Map<string, boolean>();
    }

    public assign(variable: string, value: boolean): void {
        this.data.set(variable, value);
    }

    public lookup(variable: string): boolean {
        return this.data.get(variable);
    }
}

// Abstract Expression
interface BooleanExpression {
    evaluate(context: Context): boolean;
    replace(name: string, expression: BooleanExpression): BooleanExpression;
    copy(): BooleanExpression;
}

// Variable Expression
class VariableExpression implements BooleanExpression {
    private name: string;

    constructor(name: string) {
        this.name = name;
    }

    public evaluate(context: Context): boolean {
        return context.lookup(this.name);
    }

    public replace(name: string, expression: BooleanExpression): BooleanExpression {
        if (this.name === name) {
            return expression.copy();
        } else {
            return new VariableExpression(this.name);
        }
    }

    public copy(): BooleanExpression {
        return new VariableExpression(this.name);
    }
}

// And Expression
class AndExpression implements BooleanExpression {
    private operand1: BooleanExpression;
    private operand2: BooleanExpression;

    constructor(operand1: BooleanExpression, operand2: BooleanExpression) {
        this.operand1 = operand1;
        this.operand2 = operand2;
    }

    public evaluate(context: Context): boolean {
        return this.operand1.evaluate(context) && this.operand2.evaluate(context);
    }

    public replace(name: string, expression: BooleanExpression): BooleanExpression {
        return new AndExpression(
            this.operand1.replace(name, expression),
            this.operand2.replace(name, expression)
        );
    }

    public copy(): BooleanExpression {
        return new AndExpression(this.operand1.copy(), this.operand2.copy());
    }
}

// Or Expression
class OrExpression implements BooleanExpression {
    private operand1: BooleanExpression;
    private operand2: BooleanExpression;

    constructor(operand1: BooleanExpression, operand2: BooleanExpression) {
        this.operand1 = operand1;
        this.operand2 = operand2;
    }

    public evaluate(context: Context): boolean {
        return this.operand1.evaluate(context) || this.operand2.evaluate(context);
    }

    public replace(name: string, expression: BooleanExpression): BooleanExpression {
        return new OrExpression(
            this.operand1.replace(name, expression),
            this.operand2.replace(name, expression)
        );
    }

    public copy(): BooleanExpression {
        return new OrExpression(this.operand1.copy(), this.operand2.copy());
    }
}

// Constant Expression
class ConstantExpression implements BooleanExpression {
    private constant: boolean;

    constructor(constant: boolean) {
        this.constant = constant;
    }

    public evaluate(context: Context): boolean {
        return this.constant;
    }

    public replace(name: string, expression: BooleanExpression): BooleanExpression {
        return new ConstantExpression(this.constant);
    }

    public copy(): BooleanExpression {
        return new ConstantExpression(this.constant);
    }
}

// Client code
const context = new Context();
const x = new VariableExpression('X');
const y = new VariableExpression('Y');

context.assign('X', true);
context.assign('Y', false);

const expression = new OrExpression(
    new AndExpression(new ConstantExpression(true), x),
    new AndExpression(y, new NotExpression(x))
);

console.log(expression.evaluate(context));
```

### Iterator:
```typescript
// Iterator Interface
interface Iterator<T> {
    current(): T;
    next(): T;
    key(): number;
    valid(): boolean;
    rewind(): void;
}

// Aggregate Interface
interface Aggregator {
    getIterator(): Iterator<string>;
}

// Concrete Iterator
class AlphabeticalOrderIterator implements Iterator<string> {
    private collection: WordsCollection;

    private position: number = 0;

    private reverse: boolean = false;

    constructor(collection: WordsCollection, reverse: boolean = false) {
        this.collection = collection;
        this.reverse = reverse;

        if (reverse) {
            this.position = collection.getCount() - 1;
        }
    }

    public current(): string {
        return this.collection.getItems()[this.position];
    }

    public next(): string {
        const item = this.collection.getItems()[this.position];
        this.position += this.reverse ? -1 : 1;
        return item;
    }

    public key(): number {
        return this.position;
    }

    public valid(): boolean {
        if (this.reverse) {
            return this.position >= 0;
        }

        return this.position < this.collection.getCount();
    }

    public rewind(): void {
        this.position = this.reverse ? this.collection.getCount() - 1 : 0;
    }
}

// Concrete Aggregate
class WordsCollection implements Aggregator {
    private items: string[] = [];

    public getItems(): string[] {
        return this.items;
    }

    public getCount(): number {
        return this.items.length;
    }

    public addItem(item: string): void {
        this.items.push(item);
    }

    public getIterator(): Iterator<string> {
        return new AlphabeticalOrderIterator(this);
    }

    public getReverseIterator(): Iterator<string> {
        return new AlphabeticalOrderIterator(this, true);
    }
}

// Client code
const collection = new WordsCollection();
collection.addItem('First');
collection.addItem('Second');
collection.addItem('Third');

const iterator = collection.getIterator();

console.log('Straight traversal:');
while (iterator.valid()) {
    console.log(iterator.next());
}

console.log('');
console.log('Reverse traversal:');
const reverseIterator = collection.getReverseIterator();
while (reverseIterator.valid()) {
    console.log(reverseIterator.next());
}
```

### Mediator:
```typescript
// Mediator Interface
interface Mediator {
    notify(sender: object, event: string): void;
}

// Concrete Mediator
class ConcreteMediator implements Mediator {
    private component1: Component1;
    private component2: Component2;

    constructor(c1: Component1, c2: Component2) {
        this.component1 = c1;
        this.component1.setMediator(this);
        this.component2 = c2;
        this.component2.setMediator(this);
    }

    public notify(sender: object, event: string): void {
        if (event === 'A') {
            console.log('Mediator reacts on A and triggers following operations:');
            this.component2.doC();
        }

        if (event === 'D') {
            console.log('Mediator reacts on D and triggers following operations:');
            this.component1.doB();
            this.component2.doC();
        }
    }
}

// Base Component
class BaseComponent {
    protected mediator: Mediator;

    constructor(mediator: Mediator = null) {
        this.mediator = mediator;
    }

    public setMediator(mediator: Mediator): void {
        this.mediator = mediator;
    }
}

// Concrete Components
class Component1 extends BaseComponent {
    public doA(): void {
        console.log('Component 1 does A.');
        this.mediator.notify(this, 'A');
    }

    public doB(): void {
        console.log('Component 1 does B.');
        this.mediator.notify(this, 'B');
    }
}

class Component2 extends BaseComponent {
    public doC(): void {
        console.log('Component 2 does C.');
        this.mediator.notify(this, 'C');
    }

    public doD(): void {
        console.log('Component 2 does D.');
        this.mediator.notify(this, 'D');
    }
}

// Client code
const c1 = new Component1();
const c2 = new Component2();
const mediator = new ConcreteMediator(c1, c2);

console.log('Client triggers operation A.');
c1.doA();

console.log('');
console.log('Client triggers operation D.');
c2.doD();
```

### Memento:
```typescript
// Memento Interface
interface Memento {
    getName(): string;
    getDate(): string;
}

// Concrete Memento
class ConcreteMemento implements Memento {
    private state: string;

    private date: string;

    constructor(state: string) {
        this.state = state;
        this.date = new Date().toISOString().slice(0, 19).replace('T', ' ');
    }

    public getState(): string {
        return this.state;
    }

    public getName(): string {
        return `${this.date} / (${this.state})`;
    }

    public getDate(): string {
        return this.date;
    }
}

// Originator
class Originator {
    private state: string;

    constructor(state: string) {
        this.state = state;
        console.log(`Originator: My initial state is: ${state}`);
    }

    public doSomething(): void {
        console.log('Originator: I\'m doing something important.');
        this.state = this.generateRandomString(30);
        console.log(`Originator: and my state has changed to: ${this.state}`);
    }

    private generateRandomString(length: number = 10): string {
        const charSet = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
        return Array
            .from({ length })
            .map(() => charSet.charAt(Math.floor(Math.random() * charSet.length)))
            .join('');
    }

    public save(): Memento {
        return new ConcreteMemento(this.state);
    }

    public restore(memento: Memento): void {
        this.state = (memento as ConcreteMemento).getState();
        console.log(`Originator: My state has changed to: ${this.state}`);
    }
}

// Caretaker
class Caretaker {
    private mementos: Memento[] = [];

    private originator: Originator;

    constructor(originator: Originator) {
        this.originator = originator;
    }

    public backup(): void {
        console.log('\nCaretaker: Saving Originator\'s state...');
        this.mementos.push(this.originator.save());
    }

    public undo(): void {
        if (!this.mementos.length) {
            return;
        }
        const memento = this.mementos.pop();
        console.log(`Caretaker: Restoring state to: ${memento.getName()}`);
        this.originator.restore(memento);
    }

    public showHistory(): void {
        console.log('Caretaker: Here\'s the list of mementos:');
        for (const memento of this.mementos) {
            console.log(memento.getName());
        }
    }
}

// Client code
const originator = new Originator('Super-duper-super-puper-super.');
const caretaker = new Caretaker(originator);

caretaker.backup();
originator.doSomething();

caretaker.backup();
originator.doSomething();

caretaker.backup();
originator.doSomething();

console.log('');
caretaker.showHistory();

console.log('\nClient: Now, let\'s rollback!\n');
caretaker.undo();

console.log('\nClient: Once more!\n');
caretaker.undo();
```

### Observer:
```typescript
// Observer Interface
interface Observer {
    update(subject: Subject): void;
}

// Subject Class
class Subject {
    public state: number;

    private observers: Observer[] = [];

    public attach(observer: Observer): void {
        const isExist = this.observers.includes(observer);
        if (isExist) {
            return console.log('Subject: Observer has been attached already.');
        }

        console.log('Subject: Attached an observer.');
        this.observers.push(observer);
    }

    public detach(observer: Observer): void {
        const observerIndex = this.observers.indexOf(observer);
        if (observerIndex === -1) {
            return console.log('Subject: Nonexistent observer.');
        }

        this.observers.splice(observerIndex, 1);
        console.log('Subject: Detached an observer.');
    }

    public notify(): void {
        console.log('Subject: Notifying observers...');
        for (const observer of this.observers) {
            observer.update(this);
        }
    }

    public someBusinessLogic(): void {
        console.log('\nSubject: I\'m doing something important.');
        this.state = Math.floor(Math.random() * (10 + 1));

        console.log(`Subject: My state has just changed to: ${this.state}`);
        this.notify();
    }
}

// Concrete Observers
class ConcreteObserverA implements Observer {
    public update(subject: Subject): void {
        if (subject.state < 3) {
            console.log('ConcreteObserverA: Reacted to the event.');
        }
    }
}

class ConcreteObserverB implements Observer {
    public update(subject: Subject): void {
        if (subject.state === 0 || subject.state >= 2) {
            console.log('ConcreteObserverB: Reacted to the event.');
        }
    }
}

// Client code
const subject = new Subject();

const observer1 = new ConcreteObserverA();
subject.attach(observer1);

const observer2 = new ConcreteObserverB();
subject.attach(observer2);

subject.someBusinessLogic();
subject.someBusinessLogic();

subject.detach(observer2);

subject.someBusinessLogic();
```

### State:
```typescript
// State Interface
interface State {
    handle(context: Context): void;
}

// Concrete States
class ConcreteStateA implements State {
    public handle(context: Context): void {
        console.log('ConcreteStateA handles request.');
        console.log('ConcreteStateA wants to change the state of the context.');
        context.transitionTo(new ConcreteStateB());
    }
}

class ConcreteStateB implements State {
    public handle(context: Context): void {
        console.log('ConcreteStateB handles request.');
        console.log('ConcreteStateB wants to change the state of the context.');
        context.transitionTo(new ConcreteStateA());
    }
}

// Context
class Context {
    private state: State;

    constructor(state: State) {
        this.transitionTo(state);
    }

    public transitionTo(state: State): void {
        console.log(`Context: Transition to ${(<any>state).constructor.name}.`);
        this.state = state;
    }

    public request(): void {
        this.state.handle(this);
    }
}

// Client code
const context = new Context(new ConcreteStateA());
context.request();
context.request();
context.request();
```

### Strategy:
```typescript
// Strategy Interface
interface Strategy {
    doAlgorithm(data: string[]): string[];
}

// Concrete Strategies
class ConcreteStrategyA implements Strategy {
    public doAlgorithm(data: string[]): string[] {
        return data.sort();
    }
}

class ConcreteStrategyB implements Strategy {
    public doAlgorithm(data: string[]): string[] {
        return data.reverse();
    }
}

// Context
class Context {
    private strategy: Strategy;

    constructor(strategy: Strategy) {
        this.strategy = strategy;
    }

    public setStrategy(strategy: Strategy): void {
        this.strategy = strategy;
    }

    public doSomeBusinessLogic(): void {
        const result = this.strategy.doAlgorithm(['a', 'b', 'c', 'd', 'e']);
        console.log(result.join(','));
    }
}

// Client code
const context = new Context(new ConcreteStrategyA());
console.log('Client: Strategy is set to normal sorting.');
context.doSomeBusinessLogic();

console.log('');

console.log('Client: Strategy is set to reverse sorting.');
context.setStrategy(new ConcreteStrategyB());
context.doSomeBusinessLogic();
```

### Template Method:
```typescript
// Abstract Class
abstract class AbstractClass {
    public templateMethod(): void {
        this.baseOperation1();
        this.requiredOperations1();
        this.baseOperation2();
        this.hook1();
        this.requiredOperation2();
        this.baseOperation3();
        this.hook2();
    }

    protected baseOperation1(): void {
        console.log('AbstractClass says: I am doing the bulk of the work');
    }

    protected baseOperation2(): void {
        console.log('AbstractClass says: But I let subclasses override some operations');
    }

    protected baseOperation3(): void {
        console.log('AbstractClass says: But I am doing the bulk of the work anyway');
    }

    protected abstract requiredOperations1(): void;

    protected abstract requiredOperation2(): void;

    protected hook1(): void {}

    protected hook2(): void {}
}

// Concrete Classes
class ConcreteClass1 extends AbstractClass {
    protected requiredOperations1(): void {
        console.log('ConcreteClass1 says: Implemented Operation1');
    }

    protected requiredOperation2(): void {
        console.log('ConcreteClass1 says: Implemented Operation2');
    }
}

class ConcreteClass2 extends AbstractClass {
    protected requiredOperations1(): void {
        console.log('ConcreteClass2 says: Implemented Operation1');
    }

    protected requiredOperation2(): void {
        console.log('ConcreteClass2 says: Implemented Operation2');
    }

    protected hook1(): void {
        console.log('ConcreteClass2 says: Overridden Hook1');
    }
}

// Client code
function clientCode(abstractClass: AbstractClass) {
    abstractClass.templateMethod();
}

console.log('Same client code can work with different subclasses:');
clientCode(new ConcreteClass1());
console.log('');

console.log('Same client code can work with different subclasses:');
clientCode(new ConcreteClass2());
```

### Visitor:
```typescript
// Visitor Interface
interface Visitor {
    visitConcreteComponentA(element: ConcreteComponentA): void;
    visitConcreteComponentB(element: ConcreteComponentB): void;
}

// Component Interface
interface Component {
    accept(visitor: Visitor): void;
}

// Concrete Components
class ConcreteComponentA implements Component {
    public accept(visitor: Visitor): void {
        visitor.visitConcreteComponentA(this);
    }

    public exclusiveMethodOfConcreteComponentA(): string {
        return 'A';
    }
}

class ConcreteComponentB implements Component {
    public accept(visitor: Visitor): void {
        visitor.visitConcreteComponentB(this);
    }

    public specialMethodOfConcreteComponentB(): string {
        return 'B';
    }
}

// Concrete Visitors
class ConcreteVisitor1 implements Visitor {
    public visitConcreteComponentA(element: ConcreteComponentA): void {
        console.log(`${element.exclusiveMethodOfConcreteComponentA()} + ConcreteVisitor1`);
    }

    public visitConcreteComponentB(element: ConcreteComponentB): void {
        console.log(`${element.specialMethodOfConcreteComponentB()} + ConcreteVisitor1`);
    }
}

class ConcreteVisitor2 implements Visitor {
    public visitConcreteComponentA(element: ConcreteComponentA): void {
        console.log(`${element.exclusiveMethodOfConcreteComponentA()} + ConcreteVisitor2`);
    }

    public visitConcreteComponentB(element: ConcreteComponentB): void {
        console.log(`${element.specialMethodOfConcreteComponentB()} + ConcreteVisitor2`);
    }
}

// Client code
const components = [
    new ConcreteComponentA(),
    new ConcreteComponentB(),
];

console.log('The client code works with all visitors via the base Visitor interface:');
const visitor1 = new ConcreteVisitor1();
for (const component of components) {
    component.accept(visitor1);
}

console.log('');

console.log('It allows the same client code to work with different types of visitors:');
const visitor2 = new ConcreteVisitor2();
for (const component of components) {
    component.accept(visitor2);
}
```

# Como Executar:

```bash
git clone https://github.com/LuscaKF/designPatterns.git
cd designPatterns
```

## Instalando Dependências:

```bash
npm install
```

## Executando exemplos:
```bash
ts-node src/nome-do-arquivo.ts
```

# Contribuições
### Contribuições são bem-vindas! Se você tiver sugestões de melhorias, correções de bugs ou novos exemplos de padrões de projeto, sinta-se à vontade para abrir uma issue ou enviar um pull request.

# Licença
### Este projeto está licenciado sob a licença MIT. Veja o arquivo LICENSE para mais detalhes.

```css
Sinta-se à vontade para ajustar os exemplos de código e a estrutura conforme necessário. Se precisar de mais detalhes ou ajuda com algo específico, estou à disposição!
```
