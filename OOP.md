# OOP Related Knowledge

## Coupling between object (CBO) classes
- Coupling between objects (CBO) is a count of the number of classes that are coupled to a particular class i.e. where the methods of one class `call the methods or access the variables` of the other. 
- These calls need to be **counted in both directions** so the CBO of class A is `the size of the set of classes that class A references and those classes that reference class A`.
  ![CBO](https://github.com/XinyuKang/techInterviewPrep/assets/46883505/28590513-e93f-4f99-81ee-277106271690)
- Coupling can be via attributes (composition), associations, local variables, instanciations or injected dependencies (arguments to methods).
- A high number is bad and a low number is usually good with this metric.

### Tight coupling
This scenario arises when a class assumes too many responsibilities, or when one concern is spread over many classes rather than having its own class.

Interfaces are a powerful tool to use for decoupling. Classes can communicate through interfaces `rather than other concrete classes`, and any class can be on the other end of that communication simply by implementing the interface.

[An interesting analogy](https://stackoverflow.com/questions/2832017/what-is-the-difference-between-loose-coupling-and-tight-coupling-in-the-object-o): If you change your shirt, then you are not forced to change your body - when you can do that, then you have loose coupling. When you can't do that, then you have tight coupling.

Example of tight coupling:
```
class CustomerRepository
{
    private readonly Database database;

    public CustomerRepository(Database database)
    {
        this.database = database;
    }

    public void Add(string CustomerName)
    {
        database.AddRow("Customer", CustomerName);
    }
}

class Database
{
    public void AddRow(string Table, string Value)
    {
    }
}
```

Example of loose coupling:
```
class CustomerRepository
{
    private readonly IDatabase database;

    public CustomerRepository(IDatabase database)
    {
        this.database = database;
    }

    public void Add(string CustomerName)
    {
        database.AddRow("Customer", CustomerName);
    }
}

interface IDatabase
{
    void AddRow(string Table, string Value);
}

class Database implements IDatabase
{
    public void AddRow(string Table, string Value)
    {
    }
}
```
