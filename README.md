# SOLID-Principles-Overview
# S - Single Responsibility Principle (SRP): Each class or component should have only one reason to change, meaning it should only have one job or responsibility.

In Angular:

Each service, component, or directive should only handle a single responsibility. For instance, a component responsible for displaying user data should not handle data fetching logic; instead, that logic should be in a separate service.

Example:

Before SRP:

  export class UserProfileComponent {
    user: User;

    constructor(private http: HttpClient) {
      this.http.get('/api/user').subscribe(data => this.user = data);
    }
  }


After SRP:

  export class UserProfileComponent {
    user: User;

    constructor(private userService: UserService) {
      this.userService.getUser().subscribe(data => this.user = data);
    }
  }

  @Injectable()
  export class UserService {
    constructor(private http: HttpClient) {}

    getUser() {
      return this.http.get('/api/user');
    }
  }


Real-World Use Case:
Separating data-fetching logic into services keeps the component lightweight, easier to maintain, and reusable.

# O - Open/Closed Principle (OCP): Software entities should be open for extension but closed for modification. This means you should be able to extend a class's behavior without modifying the class itself.

In Angular:

Use dependency injection and extend services to add new behaviors instead of modifying existing ones.

Example:

    export class PaymentService {
      pay(amount: number) {
      }
    }

    export class PaymentComponent {
      constructor(private paymentService: PaymentService) {}

      makePayment() {
        this.paymentService.pay(100);
      }
    }

     export class PayPalPaymentService extends PaymentService {
      pay(amount: number) {
      }
    }


Real-World Use Case:
This allows for introducing new payment methods like PayPal or cryptocurrencies by extending services without touching the core code.

# L - Liskov Substitution Principle (LSP): Subtypes should be substitutable for their base types without altering the correctness of the program.

In Angular:

When using inheritance, make sure that derived classes can replace base classes without causing errors.

Example:

    export class Animal {
      makeSound() { return 'Some sound'; }
    }

    export class Dog extends Animal {
      makeSound() { return 'Bark'; }
    }

    export class Cat extends Animal {
      makeSound() { return 'Meow'; }
    }

Real-World Use Case:
If you have an Animal service that returns a sound, any subclass like Dog or Cat can replace it without changing the app's behavior.

# I - Interface Segregation Principle (ISP): Clients should not be forced to depend on methods they do not use. It's better to have smaller, more specific interfaces than larger, general-purpose ones.

In Angular:

Use smaller, more specific interfaces that ensure components/services only depend on what they actually need.

Example:

    interface Animal {
      eat(): void;
    }

    interface Mammal extends Animal {
      walk(): void;
    }

    interface Bird extends Animal {
      fly(): void;
    }

    export class Dog implements Mammal {
      eat() { console.log('Dog is eating'); }
      walk() { console.log('Dog is walking'); }
    }


Real-World Use Case:
This ensures flexibility when developing services for different types of components in Angular, avoiding dependencies on methods that aren't required.

# D - Dependency Inversion Principle (DIP): High-level modules should not depend on low-level modules; both should depend on abstractions. Also, abstractions should not depend on details; details should depend on abstractions.

In Angular:

Use dependency injection to invert dependencies, ensuring components/services depend on abstractions (interfaces or abstract classes) rather than concrete implementations.

Example:

    export interface PaymentMethod {
      pay(amount: number): void;
    }

    @Injectable()
    export class CreditCardPaymentService implements PaymentMethod {
      pay(amount: number) {
        console.log(`Paid ${amount} with Credit Card`);
      }
    }

    @Injectable()
    export class PayPalPaymentService implements PaymentMethod {
      pay(amount: number) {
        console.log(`Paid ${amount} with PayPal`);
      }
    }

    export class PaymentComponent {
      constructor(private paymentMethod: PaymentMethod) {}

      makePayment() {
        this.paymentMethod.pay(100);
      }
    }


Real-World Use Case:
You can swap out different payment services (like credit card or PayPal) by injecting different implementations of the PaymentMethod interface.

Reference: https://stackify.com/solid-design-principles/

Github Repository: https://github.com/MJDayunot/Solid-Principles-in-Angular
