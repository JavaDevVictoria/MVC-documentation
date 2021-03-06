
MVC Design Pattern in an Object Oriented Environment

By Victoria Holland,
Java Developer.


INDEX

1.    Purpose/Overview    
2.    Introduction to design patterns    
3.    Introduction to Model View Controller   
4.    Advantages of MVC 
5.    How Polopoly uses MVC

Purpose/Overview

The aim of this document is to explain how the model view controller (MVC) design pattern works within an object oriented environment, and how Polopoly utilises MVC.

Introduction to design patterns

A design pattern describes a proven solution to a recurring design problem.  Design patterns represent the best practices used by experienced object-oriented software developers. Design patterns are solutions to general problems that software developers faced during software development. These solutions were obtained by trial and error by numerous software developers over quite a substantial period of time.

The advantages of using design patterns are as follows:
They are proven: You tap the experience, knowledge and insights of developers who have used these patterns successfully in their own work.
They are reusable: When a problem recurs, you don’t have to invent a new solution; you follow the pattern and adapt it as necessary.
They are expressive: Design patterns provide a common vocabulary of solutions, which you can use to express larger solutions succinctly.

Introduction to Model View Controller

Model–view–controller (MVC) is a software design pattern which serves to decouple data access and business logic from the manner in which it is displayed to the user. The central ideas behind MVC are code reusability and separation of content from presentation.

Diagram showing how MVC links together

The model consists of application data, the business rules that govern access to and updates of the data, business logic, and functions. In enterprise software, a model often serves as a software approximation of a real-world process.  To summarise, it handles data and business logic.

The view renders the contents of the model. It specifies how the model data should be represented, such as with a chart or a diagram. Multiple views of the same data are possible, such as a bar chart for management and a tabular view for accountants.  In summary, the view presents the data to the user in any supported format and layout.

The controller mediates input, converting it to commands for the model or view. It translates the user's interactions with the view into actions that the model will perform. In a stand-alone GUI client, user interactions could be button clicks or menu selections, whereas in an enterprise web application, they appear as GET and POST HTTP requests. Depending on the context, a controller may also select a new view -- for example, a web page of results -- to present back to the user.  So in summary, the controller receives user requests and calls appropriate resources to carry them out.

Advantages of MVC

The advantages of using MVC are as follows:
Separation of responsibilities allows flexibility further down the line. For example, because the view doesn't care about the underlying model, supporting multiple file formats is easier: just add a model subclass for each.  Separation of concerns also allows the re-use of the business logic across applications.
Developers can specialise and focus on one part of the application, for example:
The developers of the user interface can focus exclusively on the UI screens without being bogged down by the business logic.
The developer of the model can focus exclusively on the business logic implementations, modifications, updates without being concerned with the look and feel of the interface.
Parallel development by separate teams is made possible by MVC, for example:
Business logic developers can build the classes, while the UI developers can design UI screens simultaneously, resulting in interdependency and time conservation.
UI updates can be made without slowing down the business logic process.  User interfaces tend to change more frequently than business rules (different colours, fonts, screen layouts, and levels of support for new devices such as mobile phones or tablet computers). Because the model does not depend on the views, adding new types of views to the system generally does not affect the model. As a result, the scope of change is confined to the view.
Changes to the business logic can be made without requiring updates to the UI.

How Polopoly uses MVC

Model

The Java policy acts as the model.  A Policy is a plain Java class that wraps content objects. It defines data access restrictions and provides business logic connected to different kinds of content.

The output framework accessing a content item will first look at the policy bean to find an appropriate method, and then look at the content item itself.

For example, let’s say we have an article of an input template that 
has StandardArticlePolicy as its policy. Then, the statement “content.lead” could mean (in order of preference)

StandardArticlePolicy.getLead()
or
The lead field of the content item

The policy is developed in conjunction with the template. The policy class is defined in the input template. Policies execute client side. Pieces of content are automatically wrapped in Policys if accessed using the PolicyCMServerinterface

The populateDocument method in the policy class is where fields are stored and indexed in Lucene.  

The standard ElementPolicy.java class extends ContentBasePolicy.java.  This is the base policy for all policies.  As an example, it has methods to determine when content was created or modified.  The ContentBasePolicy class extends ContentPolicy.java. 

ContentPolicy.java is an automatically generated Policy implementation for com.polopoly.cm.client.Content objects. It can be used as a base class to extend when developing customised Policy classes.

The policy class is where the externalId for an article type is set.  For example, one of the article types in Polopoly is the moodboard article type, which was created especially for the English Home moodboards.  For the moodboard article type the externalId is set using:
public static final String IT_EXTERNALID = "archant.MoodboardArticle";

View

The Velocity template defines what is to be rendered on the webpage. Velocity is a simple to use, open source web framework designed to be used as the view component of an MVC architecture. 

In the Velocity template, variables are used in order to get the data from the controller, eg $content.authenticationRequired.value or $m.local.content.contentId.contentId. 

Controller

The Java controller class acts as the controller.  This processes and manipulates the information the user has entered in the Polopoly interface.

In the controller, we are provided with a writable model for the current rendering called the local model.  Variables can be set in the local model, in order to make them accessible to the Velocity output template (the view).

Most controller classes extend RenderController.java.  This is the base class for controllers in the Archant project.  It contains many useful methods, for example a utility method for checking the status of a check box in one of the layout or standard article templates.   

The populateModelBeforeCacheKey method adds data to the model.  It takes three parameters: request - a render request; m - the top model, this is the model that the render will use for rendering, and context - general controller parameters.

With regards to articles in Polopoly, the policy class only runs when an article is being created or edited, whereas the controller class runs each time an article is viewed.

