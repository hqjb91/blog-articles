# One-To-Many Relationships (Entity Framework Core)

One-to-many relationships in EF Core are fundamental to modeling relationships between entities. Below is a breakdown based on the provided notes and **Microsoft Documentation**.

---

## Required One-To-Many Relationship

### Example Entities:
```csharp
public class EAVEntity // Parent entity
{
    [Key]
    public long Id { get; set; } // Primary Key (1)
    public string Name { get; set;} = string.Empty;
    public string Description { get; set;} = string.Empty;
    // Optional collection navigation property (3)
    public ICollection<EAVAttribute> EAVAttributes { get; set; } = new List<EAVAttribute>();
}

public class EAVAttribute // Child entity
{
    [Key]
    public long Id { get; set; } // Primary Key
    public string AttributeName { get; set; } = string.Empty;
    public long EAVEntityId { get; set; } // Required foreign key property (2)
    public EAVEntity EAVEntity { get; set; } = null!; // Required reference navigation (4)
}
```

### Key Relationship Components:
1. **Principal Entity (Parent)**:  
   - Primary Key: `EAVEntity.Id` (1).
   - Optional **Collection Navigation**: `EAVEntity.EAVAttributes` (3).

2. **Dependent Entity (Child)**:  
   - Foreign Key: `EAVAttribute.EAVEntityId` (2).
   - Required **Reference Navigation**: `EAVAttribute.EAVEntity` (4).

### Behavior:
- A **required relationship** mandates that every `EAVAttribute` (child) must be related to an `EAVEntity` (parent).  
- The **foreign key (`EAVAttribute.EAVEntityId`) is not nullable**, ensuring the dependency.  
- The **parent (`EAVEntity`) can exist without children**, but a **child cannot exist without a parent**.

### EF Core Configuration:

#### Discovered by Convention:
If naming conventions and navigation properties match EF Core's expectations, the relationship will be automatically inferred.

#### Explicit Configuration:
If conventions are not met or customization is needed, you can configure the relationship explicitly.

Starting from the Parent:
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<EAVEntity>()
        .HasMany(e => e.EAVAttributes) // Navigation to children
        .WithOne(e => e.EAVEntity) // Reference to parent
        .HasForeignKey(e => e.EAVEntityId) // Foreign key
        .IsRequired(); // Ensure it's required
}
```

Starting from the Child:
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<EAVAttribute>()
        .HasOne(e => e.EAVEntity) // Reference to parent
        .WithMany(e => e.EAVAttributes) // Navigation to children
        .HasForeignKey(e => e.EAVEntityId) // Foreign key
        .IsRequired(); // Ensure it's required
}
```

---

## Optional One-To-Many Relationship

### Key Differences:
- In an **optional relationship**, the **foreign key (`EAVAttribute.EAVEntityId`) is nullable**.
- A child (`EAVAttribute`) **can exist without a parent**.

### Changes in Configuration:
For optional relationships, the **`.IsRequired(false)`** method is used.

**Explicit Configuration for Optional Relationships:**

Starting from the Parent:
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<EAVEntity>()
        .HasMany(e => e.EAVAttributes)
        .WithOne(e => e.EAVEntity)
        .HasForeignKey(e => e.EAVEntityId)
        .IsRequired(false); // Optional relationship
}
```

Starting from the Child:
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<EAVAttribute>()
        .HasOne(e => e.EAVEntity)
        .WithMany(e => e.EAVAttributes)
        .HasForeignKey(e => e.EAVEntityId)
        .IsRequired(false); // Optional relationship
}
```