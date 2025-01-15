Sure! Below are some real-world examples of using generics in TypeScript, demonstrating how they can be applied in various scenarios:

### 1. **API Response Handling**

In real-world applications, API responses can have different shapes depending on the endpoint. Generics can help ensure type safety while working with dynamic data.

```typescript
// Example of an API response structure
interface ApiResponse<T> {
    status: string;
    data: T;
    message?: string;
}

// A function to handle successful API responses
function handleApiResponse<T>(response: ApiResponse<T>) {
    if (response.status === "success") {
        console.log("Data received:", response.data);
    } else {
        console.log("Error:", response.message);
    }
}

// Example usage with a user object
interface User {
    id: number;
    name: string;
}

const userResponse: ApiResponse<User> = {
    status: "success",
    data: { id: 1, name: "Alice" }
};

handleApiResponse(userResponse);
```

Here, the `ApiResponse` type is generic, allowing you to use it with any kind of data structure (e.g., `User`, `Product`, etc.). The `handleApiResponse` function can handle various API responses in a type-safe way.

---

### 2. **Reusable Data Storage (Cache or Local Storage)**

In many applications, you might need to store data in memory or local storage. Using generics, you can create a reusable storage module that works with any type of data.

```typescript
class DataStorage<T> {
    private data: T[] = [];

    addItem(item: T) {
        this.data.push(item);
    }

    removeItem(item: T) {
        this.data = this.data.filter(i => i !== item);
    }

    getItems() {
        return [...this.data];
    }
}

// Usage with string data
const stringStorage = new DataStorage<string>();
stringStorage.addItem("Item 1");
stringStorage.addItem("Item 2");
console.log(stringStorage.getItems());  // Output: ["Item 1", "Item 2"]

// Usage with number data
const numberStorage = new DataStorage<number>();
numberStorage.addItem(42);
console.log(numberStorage.getItems());  // Output: [42]
```

This `DataStorage` class uses generics to handle any type of data, allowing you to create storage for strings, numbers, objects, or any other data type.

---

### 3. **Form Validation**

When validating forms with dynamic fields, generics can be used to ensure that each field has the correct data type.

```typescript
interface FormField<T> {
    label: string;
    value: T;
    validate: (value: T) => boolean;
}

class Form<T> {
    fields: FormField<T>[];

    constructor(fields: FormField<T>[]) {
        this.fields = fields;
    }

    validateForm(): boolean {
        return this.fields.every(field => field.validate(field.value));
    }
}

// Example: Form for user registration
const registrationForm = new Form([
    {
        label: "Username",
        value: "john_doe",
        validate: (value: string) => value.length > 3,
    },
    {
        label: "Age",
        value: 28,
        validate: (value: number) => value > 18,
    }
]);

console.log(registrationForm.validateForm());  // Output: true
```

Here, `Form` and `FormField` are generic, allowing you to validate forms with various data types (`string`, `number`, `boolean`, etc.). You can reuse the same class for multiple types of forms with different field types.

---

### 4. **Event Bus / Observer Pattern**

In event-driven architectures, generics can be used in the implementation of an event bus (or observer pattern) to ensure type safety when dealing with events.

```typescript
class EventBus {
    private listeners: { [event: string]: Function[] } = {};

    on<T>(event: string, listener: (data: T) => void): void {
        if (!this.listeners[event]) {
            this.listeners[event] = [];
        }
        this.listeners[event].push(listener);
    }

    emit<T>(event: string, data: T): void {
        if (this.listeners[event]) {
            this.listeners[event].forEach(listener => listener(data));
        }
    }
}

// Create an event bus instance
const eventBus = new EventBus();

// Subscribe to an event with a specific data type
eventBus.on<string>("userLogin", (data) => {
    console.log("User logged in with username:", data);
});

// Emit an event with specific data
eventBus.emit("userLogin", "john_doe");  // Output: User logged in with username: john_doe
```

In this example, the `EventBus` class allows you to handle events with specific types of data. This makes it possible to have strongly-typed event handlers and ensures that listeners are invoked with the correct data format.

---

### 5. **State Management**

For state management, generics can be useful when you need a flexible store that can hold different types of state.

```typescript
class Store<T> {
    private state: T;

    constructor(initialState: T) {
        this.state = initialState;
    }

    getState(): T {
        return this.state;
    }

    setState(newState: T): void {
        this.state = newState;
    }
}

// Usage: Store for user data
const userStore = new Store({ name: "Alice", age: 30 });
console.log(userStore.getState());  // Output: { name: 'Alice', age: 30 }
userStore.setState({ name: "Bob", age: 25 });
console.log(userStore.getState());  // Output: { name: 'Bob', age: 25 }

// Usage: Store for an array of items
const cartStore = new Store<string[]>(["item1", "item2"]);
console.log(cartStore.getState());  // Output: ["item1", "item2"]
cartStore.setState(["item3", "item4"]);
console.log(cartStore.getState());  // Output: ["item3", "item4"]
```

The `Store` class is a flexible solution for managing different types of state (e.g., user data, shopping cart items) using generics.

---

### 6. **Pagination for Lists**

Generics can be used for pagination, where the data type of the items in the paginated list can vary.

```typescript
interface PaginatedResponse<T> {
    items: T[];
    totalCount: number;
    currentPage: number;
    totalPages: number;
}

function fetchPage<T>(page: number, size: number): Promise<PaginatedResponse<T>> {
    // Simulating an API call
    return new Promise(resolve => {
        setTimeout(() => {
            resolve({
                items: [{ name: "Item 1" }, { name: "Item 2" }],
                totalCount: 100,
                currentPage: page,
                totalPages: 10
            });
        }, 500);
    });
}

// Usage with a list of items (type can vary)
interface Product {
    name: string;
}

fetchPage<Product>(1, 10).then(response => {
    console.log(response.items);  // Output: [{ name: "Item 1" }, { name: "Item 2" }]
});
```

Here, the `fetchPage` function can handle paginated responses with different types of data (like `Product`, `User`, etc.).

---

These are just a few real-world examples of how generics can be applied in TypeScript. The flexibility of generics makes them ideal for situations where you want to work with a variety of data types while still ensuring type safety.