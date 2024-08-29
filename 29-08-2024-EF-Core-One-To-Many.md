Notes based on Microsoft Documentation : https://learn.microsoft.com/en-us/ef/core/modeling/relationships/one-to-many
## Required One-To-Many Relationship

Example Entities :
```csharp
public class EAVEntity // Parent Entity mapped to a user defined table
{
    [Key]
    public long Id { get; set; } // Primary Key for the Entity (1)
    public string Name { get; set;} = string.Empty;
    public string Description { get; set;} = string.Empty;
    // Optional Collection Navigation to reference dependent entities (3)
    public ICollection<EAVAttribute> EAVAttributes { get; set; } = new List<EAVAttribute>();
}

public class EAVAttribute // Attribute Entity mapped to user defined columns
{
    [Key]
    public long Id { get; set; }
    public string AttributeName { get; set; } = string.Empty;
    public long EAVEntityId { get; set; } // Required foreign key property (2)
    public EAVEntity EAVEntity { get; set; } = null!; // Required reference navigation to parent entity (4)
}
```
A one-to-many relationship is made up from :
1. Primary key on the principal entity (EAVEntity.Id)
2. Foreign key on the children entities (EAVAttribute.EAVEntityId)
3. Optional collection navigation on the principal entity (EAVEntity.EVAAttributes)
4. Reference navigation on the children entities (EAVAttribute.EAVEntity)

So, for the relationship in this example:
1. The foreign key property EAVAttribute.EAVEntityId (2) is not nullable making the relationship required because every EAVAttribute must be related to some parent EAVEntity i.e. child cannot exist without a parent
2. Note that for required relationship child must always have a parent but a parent can exist without any children entities

Relationship (1), (2), (3), (4) is discovered by convention.

If these relationships are not discovered by convention we can use explicit configuration.
We can either start with the parent : 
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder) 
{ 
	modelBuilder.Entity<EAVEntity>() 
		.HasMany(e => e.EAVAttribute) 
		.WithOne(e => e.EAVEntity) 
		.HasForeignKey(e => e.EAVEntityId) 
		.IsRequired(); 
}
```

or start with the child :
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder) 
{ 
	modelBuilder.Entity<EAVAttribute>() 
		.HasOne(e => e.EAVEntity) 
		.WithMany(e => e.EAVAttribute) 
		.HasForeignKey(e => e.EAVEntityId) 
		.IsRequired(); 
}
```
## Optional One-To-Many Relationship

Optional One-To-Many Relationship is the same as above except the foreign key is now nullable and if explicit configuration is used the .IsRequired(false) is passed with the false argument.