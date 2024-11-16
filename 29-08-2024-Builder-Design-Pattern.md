# Fluent Builder Design Pattern

## Introduction

The **Fluent Builder Design Pattern** is a **creational pattern** used to create complex objects step-by-step in a readable and intuitive way. 

### Key Features:
- **Encapsulation**: Construction logic is isolated in the builder class, making the codebase cleaner and easier to maintain.
- **Fluent API**: Methods return the builder instance (`this`), enabling method chaining for better readability.
- **Flexibility**: Supports the construction of objects requiring many parameters or customizable steps.
- **Immutability**: Objects built using this pattern are often immutable post-creation.

---

## When to Use Fluent Builder

- When a class has **many optional parameters**.
- When there is a need to construct objects with **different configurations**.
- When constructors with too many arguments result in **readability issues**.

---

## Steps to Implement

1. **Define the Target Class**:
   - This is the class that will be constructed (e.g., `WorkflowDefinition` in the example).

2. **Create a Builder Class**:
   - **Hold the Target Class**: A private instance of the target class is maintained in the builder.
   - **Define Fluent Setter Methods**:
     - Each method sets a property of the target class.
     - Methods return the builder instance (`this`) to enable method chaining.
   - **Provide a `Build()` Method**:
     - Finalizes and returns the constructed object.

---

## Example: Building a WorkflowDefinition

### Target Class (`WorkflowDefinition`)
The target class represents a workflow definition with properties like ID, Name, Description, Version, and Steps.

```csharp
public class WorkflowDefinition
{
    public Guid Id { get; set; }
    public string Name { get; set; } = "";
    public string Description { get; set; } = "";
    public int Version { get; set; }
    public List<Step> Steps { get; set; } = new();
}
```

### Builder Class (`WorkflowDefinitionBuilder`)

The builder class encapsulates the logic for constructing `WorkflowDefinition` objects. It implements the following:

#### Fluent Setter Methods
These methods update specific properties of the `WorkflowDefinition` and return the builder instance:

```csharp
public IWorkflowDefinitionBuilder AddDescription(string description)
{
    _workflowDefinition.Description = description;
    return this;
}

public IWorkflowDefinitionBuilder AddName(string name)
{
    _workflowDefinition.Name = name;
    return this;
}
```

#### `Build()` Method
The `Build()` method assigns a unique `Guid` to the workflow definition and returns the completed object:

```csharp
public WorkflowDefinition Build()
{
    _workflowDefinition.Id = Guid.NewGuid();
    return _workflowDefinition;
}
```

### Complete Builder Class
```csharp
public class WorkflowDefinitionBuilder : IWorkflowDefinitionBuilder
{
    private readonly WorkflowDefinition _workflowDefinition = new();

    public IWorkflowDefinitionBuilder AddDescription(string description)
    {
        _workflowDefinition.Description = description;
        return this;
    }

    public IWorkflowDefinitionBuilder AddName(string name)
    {
        _workflowDefinition.Name = name;
        return this;
    }

    public IWorkflowDefinitionBuilder AddStep(Step step)
    {
        _workflowDefinition.Steps.Add(step);
        return this;
    }

    public IWorkflowDefinitionBuilder AddVersion(int version)
    {
        _workflowDefinition.Version = version;
        return this;
    }

    public WorkflowDefinition Build()
    {
        _workflowDefinition.Id = Guid.NewGuid();
        return _workflowDefinition;
    }
}
```

---

## Example Usage

Here’s how the builder can be used to construct a `WorkflowDefinition` object:

```csharp
var builder = new WorkflowDefinitionBuilder();

WorkflowDefinition workflow = builder
    .AddName("Approval Workflow")
    .AddDescription("A workflow for approval process")
    .AddVersion(1)
    .AddStep(new Step { Name = "Review" })
    .AddStep(new Step { Name = "Approval" })
    .Build();

Console.WriteLine($"Workflow Name: {workflow.Name}, ID: {workflow.Id}");
```

### Benefits of This Approach:
- **Readability**: The construction is intuitive and resembles natural language.
- **Customizability**: Each method focuses on setting a specific property, making it easy to adjust parameters.
- **Maintainability**: Changes to the construction logic are localized within the builder class.

---

## Summary

The **Fluent Builder Design Pattern** provides a clean and modular way to construct objects, especially when dealing with many parameters or complex configurations. By encapsulating the construction logic and offering a chainable API, this pattern improves code clarity and maintainability.