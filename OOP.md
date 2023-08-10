# OOP Related Knowledge

## Coupling between object (CBO) classes
- Coupling between objects (CBO) is a count of the number of classes that are coupled to a particular class i.e. where the methods of one class `call the methods or access the variables` of the other. 
- These calls need to be **counted in both directions** so the CBO of class A is `the size of the set of classes that class A references and those classes that reference class A`.
  ![CBO](https://github.com/XinyuKang/techInterviewPrep/assets/46883505/28590513-e93f-4f99-81ee-277106271690)
- Coupling can be via attributes (composition), associations, local variables, instanciations or injected dependencies (arguments to methods).
- A high number is bad and a low number is usually good with this metric.
