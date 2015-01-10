---
layout: post
header-img: img/default-blog-pic.jpg
---

# Groovy : Adding property in a Class by AST Transformation

Groovy provides a wonderful way of manipulating source code during compilation by AST(Abstract Syntax Tree) transformation. One can modify existing methods, add new methods, add new properties and can perform many other operations in a Class via AST Transformation. This feature can be very handy to add common code in multiple classes and prevents developer from writing duplicate code.In a way it saves developers time and reduces overall cost of the project.  In this blog I will describe how one can add new property in a Class via AST Transformation. Let's take example of Audit fields to understand the entire process of adding new properties in a Class. It is a very common practice to have following fields in a domain class for audit purpose. 

  * Created User
  * Edited User
  * Created Date
  * Edited Date

An enterprise level application can have many domain classes(mapped to table in database). During initial development adding above mentioned four fields in every class can be annoying. One might think of putting all these common fields in a Base class and extend all domain classes from it. However inheritance strategy should be used when classes logically belong to same family. Just to avoid writing duplicate code is more of technical reason of using inheritance than logical one. Also at times it is possible that classes belong to different hierarchies and still need common fields. In all such cases one can add such common properties via AST Transformation. We will do this via local AST Transformation which requires an annotation to be added on the class where required modification is to be done.

  
Following steps need to be performed for adding new fields via ASTTransformation  


**Step 1:**

Create a Class "AuditFieldsASTTransformation" which implements ASTTransformation interface and provides implementation of "visit" method. Also add Compilation phase Canonicalization(or any other as per requirement) to do the required AST transformation in given phase.

[sourcecode lang="groovy"] @GroovyASTTransformation(phase = CompilePhase.CANONICALIZATION) class AuditFieldsASTTransformation implements ASTTransformation{ public void visit(ASTNode[] nodes, SourceUnit source) { .... } } [/sourcecode] **Step 2:** Next, add logic in "visit" method to add new property in a class. 

[sourcecode lang="groovy"] @GroovyASTTransformation(phase = CompilePhase.CANONICALIZATION) class AuditFieldsASTTransformation implements ASTTransformation{ public void visit(ASTNode[] nodes, SourceUnit source) { nodes.each{ASTNode astNode-> if(astNode instanceof ClassNode){ ClassNode classNode = (ClassNode)astNode classNode.addProperty("createdBy", Modifier.PUBLIC, new ClassNode(String), null, null, null) classNode.addProperty("editedBy", Modifier.PUBLIC, new ClassNode(String), null, null, null) classNode.addProperty("createdDate", Modifier.PUBLIC, new ClassNode(Date), null, null, null) classNode.addProperty("editedDate", Modifier.PUBLIC, new ClassNode(Date), null, null, null) } } } }[/sourcecode] In above code we are adding properties "createdBy", "editedBy", "createdDate" and "editedDate" by addProperty method on ClassNode. We will see in a moment what this ClassNode actually represents. **Step 3:** Now create a custom- annotation class "Auditable" which binds above created "AuditFieldsASTTransformation" class. [sourcecode lang="groovy"] @Retention(RetentionPolicy.RUNTIME) @Target([ ElementType.TYPE ]) @GroovyASTTransformationClass("AuditFieldsASTTransformation") @interface Auditable { } [/sourcecode] In above code, we gave Target as ElementType.TYPE, which means this annotation can be applied only at Class level. After this, annotation @Auditable can be applied on any class and compiler would automatically add above mentioned four audit fields in the class at compile time. 

**Step 4:**

Let's now create a class on which audit fields are required to be added.

[sourcecode lang="groovy"] @Auditable class Project { String name String location int teamSize } [/sourcecode] Above class represents Project details. Any time, details are updated, user who updated the details and time at which details were updated needs to be persisted. So in order to add required audit fields in Project class, all we need to do is add @Auditable at class level. Now when Project class is compiled, compiler would invoke "visit" method of  "AuditFieldsASTTransformation" class. ClassNode in "visit" method represents Project class(or any other class with @Auditable annotation) and via "addProperty" method all four audit fields are added in Project class structure and finally bytecode is generated. If one decompiles bytecode of Project class and look underneath, all four audit fields can be seen in the class as mentioned below. 

[sourcecode lang="groovy"] class Project { String name String location int teamSize String createdBy String editedBy Date createdDate Date editedDate } [/sourcecode] **Conclusion** At first, adding new fields dynamically by AST Transformation at compile time might not look very exciting. But when it is combined with adding new methods in a class, one can write very generic code which can be used in multiple projects. One can add methods which automatically finds currently logged in user and updates "editedDate" field while saving domain object. This is exactly what "Audit-Trail" plugin of Grails does and can be used in any Grails project. There are many other Grails plugin available which uses this concept of adding utility fields and methods in a Class via AST Transformation. This is compile time meta-programming technique which is generally considered better than run-time meta-programming as far as performance is considered. Grails 2.0 heavily relies on this technique and adds many utility methods in various artifacts at compile time.