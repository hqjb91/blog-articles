# Fluent Builder Design Pattern

## Introduction

The **Fluent Builder Design Pattern** is a **creational pattern** used to create complex objects step-by-step in a readable and intuitive way. 

The Fluent Builder pattern is a powerful technique for creating complex objects in a clean and readable way. By using a builder class, you can encapsulate the construction logic, provide a fluent API, and enhance the maintainability of your code.

The Fluent Builder design pattern is a creational pattern used to create complex objects step by step with a clean and readable interface.

It is especially useful when dealing with objects that require multiple parameters or need to be built in various steps. 

An obvious sign that a class could benefit from this pattern is if the constructor has too many input parameters.

## Implementation

To utitlise the Fluent Builder Design Pattern :
1. We define the class that will be created (e.g. WorkflowDefinition) with the various properties.
2. We then define a builder class that will
	1. Have the class that will be created as a property of the builder class so that it can be returned by the Build() method
	2. Define Setter methods on the builder class that will set the relevant properties in the class that will be created 
	3. Important thing is these setter methods will have to **return this object** so that the methods can be chained
	4. Define a Build() method that will return the class that will be created

```
public class WorkflowDefinition
{
    public Guid Id { get; set; }
    public string Name { get; set; } = "";
    public string Description { get; set; } = "";
    public int Version { get; set; }
    public List<Step> Steps { get; set; } = [];
}

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
        Guid guid = new();
        _workflowDefinition.Id = guid;
        return _workflowDefinition;
    }
}
```