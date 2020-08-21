# RA_JavaParser_Task

## Task Timeline
The deadline of this task is 08/28/2020 18:00. We are not able to evaluate submissions after the deadline.

## How to submit task
You need to create you own git repository for the task and share the link with us via email: 
* [Xiao Wang](xwang97@stevens.edu)
* [Lu Xiao](lxiao6@stevens.edu)
* [Yutong Zhao](yzhao102@stevens.edu)

You need to create a branch for each small task and let us know (by emailing us) when you finish each part so we can evaluate the result of the task.

## Tasks
1. Config JavaParser on [Avro](https://github.com/apache/avro) project, parse three files
    * [TestResolvingGrammarGenerator.java](https://github.com/apache/avro/blob/2d3b1fe7efd865639663ba785877182e7e038c45/lang/java/avro/src/test/java/org/apache/avro/io/parsing/TestResolvingGrammarGenerator.java)
    * [TestDataFileMeta.java](https://github.com/apache/avro/blob/b534b8ba924cd7515ec71bd0c0898153952c6eba/lang/java/avro/src/test/java/org/apache/avro/TestDataFileMeta.java)
    * [TestProtocolGeneric.java](https://github.com/apache/avro/blob/2d3b1fe7efd865639663ba785877182e7e038c45/lang/java/ipc/src/test/java/org/apache/avro/TestProtocolGeneric.java)
1. Extract methods' delcaration that contain @Test annotation in each file. Design your own data structure class to save the extracted methods' declaration and provide a main function to print the qualified name of each methods.<br />
For example, in [TestDataFileMeta.java](https://github.com/apache/avro/blob/b534b8ba924cd7515ec71bd0c0898153952c6eba/lang/java/avro/src/test/java/org/apache/avro/TestDataFileMeta.java), you need to extract four methods:
    * testUseReservedMeta()
    * testUseMeta()
    * testUseMetaAfterCreate()
    * testBlockSizeSetInvalid()
1. Extract the assertion statement from each @Test method. Provide a main function to print the assertion statement.
1. Extract the methods’ qualified name invoked in the assertion statement. Provide a main function to print the methods' qualified name.

## JavaParser
JavaParser is a java library that allows users to interact with java source code. It parses source code into AST tree and provides visitor patterns for users to override and visit various software entities. More detail could be find here: [JavaParserVisited.pdf](https://drive.google.com/file/d/1xNlA4ZVsbySxIkXBnVAd-0vxIAnxDnMP/view?usp=sharing)

## Configure JavaParser and JavaSymbolSolver
You can follow these steps to config JavaParser and parse your own project:
1. Initializing CombinedTypeSolver, add src roots and required jar files into solver.
```java
private void initCombinedSolver() throws IOException {
  CombinedTypeSolver combinedSolver = new CombinedTypeSolver();
  TypeSolver reflectionTypeSolver = new ReflectionTypeSolver();
  // get src root
  final ProjectRoot projectRoot = new SymbolSolverCollectionStrategy().collect(Paths.get(src_dir));
  List<SourceRoot> sourceRootList = projectRoot.getSourceRoots();
  // add java jre solver
  combinedSolver.add(reflectionTypeSolver);
  // add src roots into combined solver
  for (SourceRoot sourceRoot : sourceRootList) {
    combinedSolver.add(new JavaParserTypeSolver(sourceRoot.getRoot()));
  }
  // add jar solver
  if (jar_dir != null) {
    for (File jar_File : new File(jar_dir).listFiles()) {
      combinedSolver.add(new JarTypeSolver(jar_File));
    }
  }
  this.combinedSolver = combinedSolver;
}
```
2. Overriding visitor functions to visit specific software entities and extract information.
```java
private class MethodCallExpressionVisitor extends VoidVisitorAdapter<Void> {
  @Override
  public void visit(MethodCallExpr mce, Void arg) {
    System.out.println("======================");
    System.out.println("Method Call Expression: \n" + mce);
    try {
      mce.resolve();
    } catch (Exception e) {
      System.err.println(e);
    }

  }
}
```
Detailed software entity information could be found in [JavaParserVisited.pdf](https://drive.google.com/file/d/1xNlA4ZVsbySxIkXBnVAd-0vxIAnxDnMP/view?usp=sharing) Appendix B, Page 61.
