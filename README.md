
# Clean Architecture

A beginning stage for Clean Architecture with ASP.NET Core. Clean Architecture is only the most recent in a progression of names for the equivalent approximately coupled, reliance transformed design. You will likewise discover it named hexagonal, ports-and-connectors, or onion design.

## Give a Star! :star:
If you like or are using this project to learn or start your solution, please give it a star. Thanks!

# Goals

The objective of this archive is to give a fundamental arrangement structure that can be utilized to construct Domain-Driven Design (DDD)- based or essentially well-calculated, SOLID applications utilizing .NET Core.

# Design Decisions and Dependencies

The objective of this example is to give a genuinely stripped down starter pack for new activities. It does exclude each conceivable system, instrument, or highlight that a specific endeavor application may profit by. Its decisions of innovation for things like information get to are established in what is the most well-known, available innovation for most business programming engineers utilizing Microsoft's innovation stack. It doesn't (as of now) incorporate broad help for things like logging, observing, or examination, however these would all be able to be included effectively. The following is a rundown of the innovation conditions it incorporates, and why they were picked. The majority of these can without much of a stretch be swapped out for your innovation of decision, since the idea of this engineering is to help seclusion and embodiment or Encapsulation.


## The Core Project

The Core venture is the focal point of the Clean Architecture structure, and all other task conditions should point toward it. In that capacity, it has not many outer conditions. The one special case for this situation is the System.Reflection.TypeExtensions bundle, which is utilized by ValueObject to help execute its IEquatable<> interface. The Core task ought to incorporate things like:
- Entities
- Aggregates
- Domain Events
- DTOs
- Interfaces
- Event Handlers
- Domain Services
- Specifications
Numerous arrangements will likewise reference a different Shared Kernel venture/bundle. I suggest making a different SharedKernel undertaking and arrangement in the event that you will require sharing code between various activities. I further suggest this be distributed as a nuget bundle (almost certain secretly) and referenced as a nuget reliance by those undertakings that require it. For this example, in light of a legitimate concern for effortlessness, I've added a SharedKernel organizer to the Core venture which contains types that would probably be shared between different tasks, in my experience.


## The Infrastructure Project

The vast majority of your application's conditions on outer assets ought to be actualized in classes characterized in the Infrastructure venture. These classes should execute interfaces characterized in Core. On the off chance that you have a huge venture with numerous conditions, it might bode well to have various Infrastructure ventures (for example Infrastructure.Data), yet for most undertakings one Infrastructure venture with organizers works fine. The example incorporates information access and area occasion executions, yet you would likewise include things like email suppliers, record get to, web programming interface customers, and so forth to this task so they're not adding coupling to your Core or UI ventures. 
 
The Infrastructure venture relies upon Microsoft.EntityFrameworkCore.SqlServer and Autofac. The previous is utilized in light of the fact that it's incorporated with the default ASP.NET Core layouts and is the lowest shared factor of information get to. Whenever wanted, it can without much of a stretch be supplanted with a lighter-weight ORM like Dapper. Autofac (in the past StructureMap) is utilized to permit wire up of conditions to happen nearest to where the usage live. For this situation, an InfrastructureRegistry class can be utilized in the Infrastructure class to permit wire up of conditions there, without the section purpose of the application in any event, needing a reference to the venture or its sorts. The present execution does exclude this conduct - it's something I regularly cover and have understudies include themselves in my workshops.

## The Web Project

The section purpose of the application is the ASP.NET Core web venture. This is really a support application, with an open static void Main strategy in Program.cs. It as of now utilizes the default MVC association (Controllers and Views envelopes) just as the vast majority of the default ASP.NET Core venture layout code. This incorporates its setup framework, which utilizes the default appsettings.json document in addition to condition factors, and is designed in Startup.cs. The venture agents to the Infrastructure task to wire up its administrations utilizing Autofac.

## The Test Projects

Test projects could be organized based on the kind of test (unit, functional, integration, performance, etc.) or by the project they are testing (Core, Infrastructure, Web), or both. For this simple starter kit, the test projects are organized based on the kind of test, with unit, functional and integration test projects existing in this solution. In terms of dependencies, there are three worth noting:

- [xunit](https://www.nuget.org/packages/xunit)  I'm utilizing xunit on the grounds that that is the thing that ASP.NET Core utilizes inside to test the item. It works incredible and as new forms of ASP.NET Core send, I'm sure it will keep on functioning admirably with it.

- [Moq](https://www.nuget.org/packages/Moq/) I'm utilizing Moq as a ridiculing structure for white box conduct based tests. In the event that I have a strategy that, in specific situations, ought to play out an activity that isn't clear from the item's recognizable state, ridicules give an approach to test that. I could likewise utilize my own Fake execution, however that requires much more composing and documents. Moq is incredible once you get the hang, and expecting you don't need to ridicule the world (which we don't for this situation on account of good, secluded plan).

- [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) I'm utilizing TestHost to test my web venture utilizing its full stack, not simply unit testing activity strategies. Utilizing TestHost, you make real HttpClient demands without going over the wire (so no firewall or port setup issues). Tests run in memory and are quick, and solicitations practice the full MVC stack, including steering, model authoritative, model approval, channels, and so forth.


