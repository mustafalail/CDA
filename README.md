# Class Diragram Analyzer (CDA)

## CDA Overview and Demo
We have created a video demonstration of CDA that can be viewed below or through YouTube CDA Demonstration.

## CDA Installation Instructions
This repository contains a modified distribution of USE that includes our CDA plug-in. To install the tool, download the repository files as a zip folder and extract them in the desired install directory.

Note: You must install the Java Runtime Environment in order to run USE.

## Overview of USE
We have used MDE technologies to define, implement, and package the code of the CDA tool as a plugin for the UML-based Specification Environment (USE). We provide a video overview of how to create UML design class diagrams and load them into USE for analysis. This tutorial can be watched below and can also be acessed on [YouTube](https://www.youtube.com/watch?v=44PB0AZTZjA).


https://github.com/user-attachments/assets/e8fe7339-aada-407a-a080-e13d08023f37



## Running CDA
### Running USE on Windows

Open the bin folder and run the ‘use’ executable.

### Running USE on Mac

Open a terminal window.
Traverse to the lib directory of the downloaded USE files
Execute the following command: java -jar use.jar
Launching CDA

After running USE, a user needs to open the CDA plugin by clicking the icon shown below.

USEMainWindow

Figure 1. USE Main Window

This opens the CDA Main Window shown below:

MainWindow

Figure 2. CDA Main Window

## A Brief Tour of CDA
In the following, we present a brief "tour" of the CDA tool. The purpose of this tour is to quickly give a demonstration of the functionality of the tool and to introduce new users to its workflow.

### Part 1: Modeling System Structure
CDA utlizes UML class diagrams as its primary modeling artifacts. Users can create UML diagrams in on of two ways.

Firstly, because CDA is built on top of the USE tool, users can specify class diagrams in the USE specification format. They can do this by creating a .use file then importing it into USE, or by utilizing the built in file editor in the UI.

Users may also create class diagrams outside of CDA and USE entirely and opt to use a dedicated modeling tool such as Papyrus. Users can then import an XMI format .uml from the other tool into USE.

In this tour we will specify a class diagram for a simple TrafficLight system. This system models the relationship between traffic light and pedestrian crossing buttons. We can begin to specify a model for this system by opening USE, opening the file editor, and writing the folowing stump.

Stump of TrafficLight system specification

Figure 2. Stump of TrafficLight system specification.

After saving the specification, we can view its correponding class diagram generated by USE. We can see that the diagram contains a TrafficLight class and a Button class to represent traffic lights and pedestrian buttons respectively. The classes are also associated through the TrafficLightButton association, which allows TrafficLight objects to be linked to many Button objects.

Basic skeleton of the TrafficLight diagram

Figure 3. Basic skeleton of the TrafficLight diagram.

Of course, this diagram is much too basic. We can expand it by reopening our specification, and writing a couple more lines as shown below.

Structural specification of the TrafficLight system

Figure 4. Structural specification of the TrafficLight system.

We have now added the attributes and values necessary to represent car and pedetrian light states. In addition, we have an attribute to kee track of whether a light has been requested to change by a pedestrian button. This specification produces the following diagram when loaded in USE.

Class diagram of the TrafficLight system

Figure 5. Class diagram of the TrafficLight system.

The class diagram we have created offers a good structural representation of our system. In the following section, we will utilize CDA in order to verify whether the structures we have created can be insantiated as we expect.

### Part 2: Structural Simulation
Having created a class diagram sucessfully, we can use CDA to perform structural analysis on our design. To do this, we first open the CDA window and the structural analysis panel. We then make sure the analysis mode for the tool is set to "Simulation".

CDA structural analysis window

Figure 6. The CDA structural analysis window.

In order to instantiate our model, we will need to define what we want our instance to look like. We can specify this as an OCL invariant as shown in Figure 6. The invariant we have specified states that we want to generate a system state that contains a traffic light with a green car light.

At the bottom of the CDA window, there is a section for analysis configuration. By default, CDA abstracts the otherwise complex task of setting many model analysis variables into one simple 'search-scope' variable. The analysis search scope defines the maximum number of instances of each class that can exist in a given system state. This sets an upper bound for the number of system states that will be explored in order to find a state that satisfies our model and invariant specification.

After clicking the simulate button, CDA analyses our model and constraints within the search scope we have defined and tries to find one possible system state that satisfies our requirements. If CDA finds such an example, it generates an object diagram like the one shown in Figure 7. This diagram consists of one TrafficLight object and one Button object linked together.

Object diagram

Figure 7. Object diagram of a system state satisfying the analysis parameters of Figure 6.

We ensured that our model can be instantiated as expected. However, we have only specified a structure for our system. Its behavior is still undefined. In the next part of the tour, we will look at how we can specify system behavior in CDA.

### Part 3: Specifying System Behavior
To create a behavioral specification for our syste we can perform two tasks. First, we define class operations that will model the kinds of events that can occur in the system. Here, we define the operations switchPedLight and switchCarLight on the TrafficLight class and the operation requestPass on the button class. To describe the behavior of each of these operations we utilize OCL constraints. By using OCL pre- and postconditions, we specify how each operation should affect the current system state when executed. Figure 8 shows the resulting specification.

Behavioral specification of the system

Figure 8. Structural and behavioral specification of the system.

We can then employ CDA to create an instance of valid system behavior.

### Part 4: Simulating System Behavior
When we simulate system behavior in CDA, the tool searches for a "scenario" that demonstrates the behavior we are looking for. The user has multiple ways of being presented to the user, as we will see soon. For now, we open the CDA window as we did when simulating system structure, except we now navigate to the behavioral analysis panel and appropriately select simulation as the analysis mode for the tool as shown below.

Behavioral specification of the system

Figure 9. CDA behavioral analysis window.

We must now specify what kind of behavior we want CDA to simulate. When performing behavioral analysis, CDA allows users to write expressions using TOCL, an extension of OCL for specifying temporal constraints. These expresions are called temporal properties and describe how the system should behave across many states in time. In this case, we specify that CDA should simulate a scenario in which a pedestrian light changes to green in response to a request by a pedestrian button. We user the TOCL operator next to express that the switch should occur the next state after which a request is recieved.

Like with structural analysis, CDA abstracts the complex work of setting many analysis variables into a more managable two analysis parameters: search-scope and search-depth. The scope parameter defines the same constraints it does in the structural analysis mode explained earlier. The depth parameter specifies how many events can take place within a simulated scenario. Like the scope parameter, search-depth places an upper bound on the search space and ensures that analysis times are not impractically long due to having to search every possible system state.

After clicking the simulate button, CDA performs its analysis and looks for a scenario that satisfies the specification we created. Once found, it generates an object diagram representing the scenario shown in Figure 10.

STM scenario

Figure 10. Scenario simulating the desired behavior.

When performing behavioral analysis, CDA utilizes a specialized kind of object diagram called a Snapshot Transition Model (STM). STMs provide a way of presenting a detailed system scenario consisting of multiple states in a familiar object diagram syntax. Figure 10 is a scenario that features two system states, which are represented by the snapshot objects s1 and s2. These states are linked by the trafficlight_switchpedlight object, which represents the execution of the switchPedLight operation we defined in Part 3 of the tour. The sets of objects linked to s1 and s2 represent the state of the system before and after the operation took place respectively. We can see that, in the first system state s1, a traffic light has been requested to let a pedestrian cross. From this state, the system triggers an operation taht switches the pedestrian light to green. In the following state s2, the pedestrian light is set to green. This scenario demonstrates the behavior that we specified in the CDA window.

The STM representation of a scenario can be intimidating for users, as it presents detailed information about each state that can overwhelm the user at first sight. For this reason, CDA also allows users to view a scenario in the form of a UML sequence diagram. Sequence diagrams are useful because they portray scenarios as a series of operation executions on individual objects. This makes it very easy for users to understand the general sequence of events before dwelving into the details of each system state. Figure 11 shows a sequence diagram of the scenario from Figure 10.

STM scenario

Figure 11. Sequence diagram of the traffic light scenario.

### Part 5: Validating System Behavior
We have now seen that the model we specified can simulate the behavior we want. However, suppose that we want the temporal property we specified earlier, that a pedestrian light should change as soon as it is requested, to hold for every possible scenario. How can we ensure that the property is not violated by the system? The answer is that we can utilize CDA to validate whether a property is violated across a specified search space. Once more, we open the CDA window and specify the same property we did before, except this time we set CDA to "Validation" mode.

CDA window

Figure 12. Analysis configuration for validation of behavior.

When performing validation, CDA searches for a "counterexample" to the specified temporal property. The counterexample consists of a scenario demonstrating behavior that violates the property. For example, Figure 13 shows a counterexample generated by CDA that shows it is possible to violate the property we defined previously; Figure 14 presents its corresponding sequence diagram.

STM scenario

Figure 13. Counterexample for the temporal property.

Sequence diagram

Figure 14. Sequence diagram of the counterexample.

This scenario presents a trace of valid system execution in which a light is requested but does not respond by changing its pedestrian light to green. Once we identify that a violation of the property is permitted by our design, we can take steps to address it. In this case, we can specify an additional precondition for the switchCarLight operation that prevents it from switching the light to green if the pedestrian light is requested. This should give priority to pedestrian crossing request and satisfy our temporal property.

Running the analyis again, we can see that the analysis finds no scenario that can violate the property. Of course, it is important to remember that the search-space is limited by our search parameters and users can increment the search-space if they wish to increase their confidence that there is no possible state in which the property is violated.

### Part 6: End of Tour
In this tour, we covered the majority of the functionality offered by CDA. Nevertheless, this tour is not exhaustive and there are some details and advanced options which cannot be practically summarized in what is supposed to be a brief overview. Therefore, we invite users to explore advanced configurations and features of CDA once they are familiar with the tool. Both by experimenting independently and by reading the papers dwelving deep into the technical details behind CDA.

Along with the tool, we have provided a few examples in the examples folder of this repository including a Traffic Light model and a Steam Boiler Control System Model, both with temporal properties.

Traffic light example folder

Figure 15. TrafficLight example folder

This folder includes a .uml file of the model and a folder of temporal properties.

## CDA Evaluation
### Evaluation Methodology

To evaluate CDA, we used a survey based on the innovative user needs experience (NX) evaluation method proposed by Zarour, 2020. The method is based on three pillars:

1. Evaluation theory from the social sciences.

2. The ISO/IEC SQuaRE 25000 series standards.

3. The NX dimension of the UX framework.

The survey contains a total of 44 questions grouped into four sections focusing on:

**Usefulness**: How useful do you find CDA?

**Pleasure**: How much do you enjoy using CDA

**Aesthetics**: How appealing do you find the user interface of CDA?

**Trust**: How much do you trust CDA to be reliable and secure?

After each group of questions, the survey includes a section where users can provide qualitative feedback on each section or suggestions for improvement. To get insights into the user experience of CDA, we collected responses from a sample of student participants in the software design and software engineering courses at Texas A&M International University. We wanted to make the study as realistic as possible for actual CDA users, so we did not provide the students with formal training on MDE, model checking, or temporal property specification. Instead, we instructed the students to download CDA from this GitHub repository, watch a few tools overview videos, and follow the instructions to download and run the tool. Additionally, we provided the students with an evaluation guide that gave them some basic training on software specification and validation and two specification and validation case studies that they should complete before responding to the survey questions. We gave the students two weeks to complete the survey independently, without supervision. A total of 19 student participants responded to the study.

### Evaluation Guide

We have created the following linked simple document to guide through the evaluation process--[CDA Evaluation Guide](https://docs.google.com/document/d/1otBNhrkt0rW8WY37WpMNodvuUxJDazzzdPBzR4oBwFg/). After completing the tasks in the guide, the students completed the survey and gave ratings of each question.

**Survey Questions**

The actual questions of the survey can be accessed through the following link (Questions.)

**Evaluation Participation**

We would love to hear your feedback about CDA. If you would like to provide your feedback, you can fill out the following survey at CDA Survey. Your thoughtful feedback is greatly appreciated.

## References
1. Al-Lail, Mustafa, Ramadan Abdunabi, Robert B. France, and Indrakshi Ray. "An Approach to Analyzing Temporal Properties in UML Class Models." In MoDeVVa@ MoDELS, pp. 77-86. 2013.
2. Al-Lail, Mustafa. "A Framework for Specifying and Analyzing Temporal Properties of UML Class Models." In MoDELS (Demos/Posters/StudentResearch), pp. 112-117. 2013.
3. Al-Lail, Mustafa, Ramadan Abdunabi, Robert B. France, and Indrakshi Ray. "Rigorous Analysis of Temporal Access Control Properties in Mobile Systems." In 2013 18th International Conference on Engineering of Complex Computer Systems, pp. 246-251. IEEE, 2013.
4. Al-Lail, Mustafa, Wuliang Sun, and Robert B. France. "Analyzing behavioral aspects of UML design class models against temporal properties." In 2014 14th International Conference on Quality Software, pp. 196-201. IEEE, 2014.
5. Al Lail, Mustafa. "A Unified Modeling Language Framework for Specifying and Analyzing Temporal Properties." PhD diss., Colorado State University, 2018.
6. Al Lail, Mustafa, Antonio Rosales, Hector Cardenas, Lars Hamann, and Alfredo Perez. "Transformation of TOCL temporal properties into OCL." In Proceedings of the 25th International Conference on Model Driven Engineering Languages and Systems: Companion Proceedings, pp. 899-907. 2022.
7. Cardenas, Hector, Zimmerman Ryan, Antonio Rosales Viesca, Mustafa Al Lail, and Alfredo J. Perez. "Formal UML-based Modeling and Analysis for Securing Location-based IoT Applications." In REUNS 2022: The 8th National Workshop for REU Research in Networking and Systems. 2022.
8. Cardenas, Hector and Mustafa Al Lail. "Specifying Temporal Properties in UML Using Patterns: A Tool-supported Approach." In Proceedings of the 26th International Conference on Model Driven Engineering Languages and Systems: Companion Proceedings. 2023.
9. Rosales Viesca, Antonio and Mustafa Al Lail. "Automated Mitigation of Frame Problem in UML Class Diagram Verification." In Proceedings of the 26th International Conference on Model Driven Engineering Languages and Systems: Companion Proceedings. 2023.
10. Viesca, Antonio Rosales, Mustafa Al Lail, and Omar Alam. "Streamlining CPS Validation: Using Interoperable UML Tools for Seamless Model Exchange." In 2024 IEEE 27th International Symposium on Real-Time Distributed Computing (ISORC), pp. 1-4. IEEE, 2024.
11. Viesca, A. R., & Al Lail, M. (2024, September). Taming the Frame Problem– An Automated Approach for Robust UML Class Diagram Specification and Verification. Innovations in Systems and Software Engineering. Springer Nature.
