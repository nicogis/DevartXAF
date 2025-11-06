# 🛠 Custom T4 Template Setup for Devart EF Core with DevExpress XAF

## ✅ Requirements

Before you start, make sure you have the following installed:

- **DevExpress XAF (eXpressApp Framework) (25.1.x)** 
  - [DevExpress XAF](https://www.devexpress.com/products/net/application_framework/)

- **Devart Entity Developer (Edition for Entity Framework) (7.3.235)**
  - [Devart Entity Developer](https://www.devart.com/entitydeveloper/entity-framework-core-designer.html)

---

## 📁 Template Location

There are two files:
- `.tmpl` (T4 template file)
- `.xml` (metadata)

Copy the two files into the folder located at:

```
C:\Program Files (x86)\Common Files\Devart\EntityDeveloper\Shared Templates
```


---

## 🔧 Step-by-Step Instructions

1. **Create an app using the XAF wizard.**

2. **Save, close, and reopen the project.**

3. **Create a new Devart EF Core model file.**
     - Right-click > Add > New Item
     - Choose: `Data` or `ASP.NET Core > Data`
     - Select: **Devart EF Core Model**

4. **Select "Database First" approach.**

5. **Configure the SQL Server connection.**
   - Choose server and database.
   - In **Advanced**, set the following:
     ```
     MultipleActiveResultSets=True
     Encrypt=True
     TrustServerCertificate=True
     ```

6. **Generate model from database.**
   - Select the desired tables or select them later.

7. **Leave the default settings**
  
8. **Configure context and namespace:**
    - Namespace context/entities: &lt;YourNameProject&gt;.Module.BusinessObjects
    - Context Class Name: &lt;YourNameProject&gt;EFCoreDbContext
  
9. **Add the custom template:**
    - Click the first "Add Template" button
    - From `Shared`, choose: **EF Core XAF**
    - Remove any other existing template

10. **Click "Finish" to complete the setup.**

11. **Change code in XAF wizard**
```csharp
//[TypesInfoInitializer(typeof(TelemacoContextInitializer))]
public partial class TelemacoEFCoreDbContext : DbContext {
    //public TelemacoEFCoreDbContext(DbContextOptions<TelemacoEFCoreDbContext> options) : base(options) {
    //}
    //public DbSet<ModuleInfo> ModulesInfo { get; set; }

    partial void OnModelCreatingPartial(ModelBuilder modelBuilder)
    {
        modelBuilder.UseDeferredDeletion(this);
        modelBuilder.UseOptimisticLock();
        modelBuilder.SetOneToManyAssociationDeleteBehavior(DeleteBehavior.SetNull, DeleteBehavior.Cascade);
        modelBuilder.HasChangeTrackingStrategy(ChangeTrackingStrategy.ChangingAndChangedNotificationsWithOriginalValues);
        modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
    }
    
}
```
     

---

## ✅ Result

Your Devart EF Core model will now generate code that’s fully compatible with DevExpress XAF using the customized T4 template.


