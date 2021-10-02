
# RASD
   ## 1 Introduction
   ### 1.1 Purpose of this document
   The purpose of a **R**equirement **A**nalysis   and **S**pecifications **D**ocument is the
process of discovering the purpose for which a software system was intended,
by identifying stakeholders and their needs, and documenting these in a form
that is amenable to analysis, communication, and subsequent implementation.
It is also concerned with the relationship of software's factors such as goals,
functions and constrains to precise specifications of software behavior, and to
their evolution over time and across software families.
   ### 1.2 Actual System
   The system we are to develop is brand new, there is no similar previous system
   ### 1.3 Purpose
 
SafeStreets is a crowd-sourced application which aim at providing cooperation among citizens (end users) and autorithies (municipality,local police ecc.) in order to make more vivable our urban environments.
Indeed, by means of SafeStreets users can notify autorithies when traffic violations occur,in particular parking violations. The application allows users to send pictures of the various type of violations(veichles parked in the middle of bike lines or in places reserved for people with disabilities,double parking, and so on),including their date,time,position and the type of violation.
SafeStreets stores the information provided by users, completing it with suitable metadata, in particular when it receives a picture, it runs an algorithm to read the license plate,the license plate may also be provided by the user during the reporting if the environment condition are not favorable (insufficient lighting).
The information provided by users can also be used (by authorities, whose level of visibility may vary in the context of the application) for statistic purposes,e.g. highlighing the streets (or areas) with the highest frequency of violations, or the veichles that commit the most violations or the type of violations for a specific street.
SafeStreets can even cooperate with municipality's services (if available), crossing the information about incidents in the territory of the municipality with its own stored data to identify potentially unsafe areas and suggest possible interventions in order to prevent possible future incidents or violations.
SafeStreets is intended to be a means to help people becoming more civilized improving the efficience of authorities to guarantee the respect of the traffic laws, the municipality (and in particular local police) could use the data stored in SafeStreets about traffic violations to generate traffic tickets. In this case, the authenticity of the stored data must be guaranteed, if it is not the data must be discarded. From the opposite side, SafeStreets can use the information about the generation of traffic tickets to build statitics about the effectiveness of SafeStreets initiative.
#### 1.3.1 Goals
- G1: **Allow the users to send data about traffic violations**
- G2: **Allow the authorities to access to violation data**
- G3: **Allow the authorities to see specific kind of statistics about traffic violations**
- G4: **Allow users to see specific kind of statistics about traffic violations**
- G5: **Allow the local police to use the data stored regarding the traffic violation to issue traffic tickets**
- G6: **Allow to exploit the traffic ticket statistics to check its effectiveness**
- G7: **Allow to cross information between the information of incidents in the area that belong to a municipality and data stored in the system to suggest possible interventions to municipalities**

### 1.4 Scope
//TODO
sul RASD di esempio di github purpose non c'è e scope è quello che abbiamo scritto su purpose 
### 1.5 Glossary
#### 1.5.1 Definitions
**System**: the SafeStreets software we are to develop

**Guest or Guest User**: the person who access the system as no logged user

**Logged user or Authenticated user**: authenticated person (not authority) who is interfacing with the system

**User** guest user or logged user in general

**Banned User** A logged User or Authenticated user that cannot interface with the system anymore due to violations of terms and conditions of SafeStreets

**Authority User or Autheticated authority**: The authenticated public organization which use Safestreet

**Local Police or Police**: The department of a local police

**Registration**: when user provide all the necessary data for accounting him into the system

**Violation or Traffic violation** a violation of the traffic laws in general


#### 1.5.2 Acronyms
**RASD**: Requirements Analysis and Specification Document
**OCR** Optic character recognition
**DBMS**: Data Base Management System
**DB**: Database
**API or APIs**: Application Programming Interface
**GPS**: Global Position System
**S2B**: Software to be
#### 1.5.3 Abbreviations
**i.e.**: that is
**w.r.t.**: with respect to
**i.d.**: id est
**i.f.f.**: if and only if
**e.g.**: exempli gratia
**etc.**: et cetera
### 1.6 Document Overview
According to the IEEE standard[2], this document is structured as
1. **Introduction**: it provides an overview of this entire document and product goals
2. **Overall description**: it describes general factors that affect the product providing the background for system requirements
3. **Specific requirements**: it contains all system's functional and nonfunctional requirements
4. **Use cases identification**: it contains the usage scenarios of the system with the use case diagram, use cases descriptions and other diagrams
5.  **Appendices**: it contains the Alloy model, software and tools used, hours of work per each team member SafeStreets

### 1.7 Reference documents
- [1]Specification document: “Mandatory Project Assignment AY2019‐2020”
- [2]IEEE Std 830‐1998 IEEE Recommended Practice for Software Requirements Specifications
- [3]UML diagrams: https://www.uml‐diagrams.org/
- [4]Alloy doc: http://alloy.lcs.mit.edu/alloy/documentation/quickguide/seq.html


## 2. Overall description
### 2.1 Product perspective
As stated before the S2B : SafeStreets, provides different services with slight variations depending on which kind of logged user is interfacing with the system, while others types of services are provided only at certain kind of logged users. The system offers the basic service which is collecting the information about a traffic violations provided by logged users(not authorities or local police and hence Authority users) and shows statistics about them, the information inserted by logged users are the type of violations,location,date,time,etc. and for what concern the location the system is able to exploit the device's available sensors and GPS toghether with the **Google Maps APIs**. Google Maps has been chosen instead of other mapping and location provider since it offers a very large library of APIs with extensive documentation and so it will be easier to exploit these functionalities and also to add possible future functionalities that have to exploit these mapping services into our system.
The Google Maps APIs are even used in the basic service for providing statistics about traffic violations (as mentioned before,e.g., when highlighting a street with the highest frequency of a certain type of traffic violation we do that using the Google Map APIs).
Moreover,in the basic service, the S2B on receiving new data from a logged user, it runs an alogorithm to read the license plate of the veichle from the picture, even though it seems an easy job it may be very complex, therefore SafeStreets exploit an external algorithm for doing that, **OCR - Optic Character Recognition**. The OCR is a well known algorithm that allows to capture characters of a license plate (in this case) from a picture, to reduce the amount of error it apply several filters to discard character that don't belong to the license plate.
For what concern the specific services provided by SafeStreets at the authority users (e.g. suggest interventions in unsafe areas )the system need to exploit the information about incidents in a certain territory, these **information** are provided by the **municipalities** and without them the S2B cannot provide the service.
The S2B must guarantee the **authenticity** of the data stored, this situation is different than the mentioned before, in fact, for doing so we will not use external functionalities because the programming languange used in the S2B already provides a mechanisism for guaranteeing it.
The below high‐level class diagram provides a model of the application domain: it contains only few essential attributes for the various classes and does not include every class that will be necessary to define the model(useful data)of the system.

---


![figura 1: Class Diagram](./images/class_diagram.png)

Vehicles commit traffic violations in different part of the globe that belongs to different municipalities and passers (if they are registrated to our S2B) can notify the violation using the SafeStreets application,the license plates of the vehicles are read using an external library that uses the OCR algorithm or provided by the users if the OCR fails,moreover logged users can see statistics about traffic violations, the statistics exploit (as mentioned before)the Google Map APIs. Authorities (including municipalities and local police) can see statistics with a different level of visibility (even here Google Map APIs are exploited) and exploiting the data concerning incidents provided by the municipalities and data stored in the SafeStreets system they can also see the possible interventions to prevent future incidents suggested by SafeStreets.
The Local police can generate traffic tickets exploiting the traffic violations reported by the user and stored in the system.
At the end SafeStreet can exploit the statistics about the generation of traffic tickets by the Local Police to know the real effectiveness of their services.
Now we are going to analyze some critical aspects of the application, modeling their behaviours and their evolution states by means of the state diagrams reported below.
The first state diagram represents the insertion by a logged user of a traffic violation in our system. As mentioned before we need to start an external algorithm (OCR) to read the license plate from a picture, the license plate is showed to the user that have to confirm or modify it (data confirmation) then we can reach our final state and the data are correctly stored.

![](./images/state_diagram_1.png)
figura 2: Data Insertion

---

The second state diagram represents the generation of a traffic ticket by an authority user (type Local Police).
This state diagram is a consequence of the previous, in fact, the local police exploit the data stored in SafeStreets to issue traffic tickets.

![](./images/state_diagram_2.png)
figura 3: Traffic Ticket Generation

---

As said in the **D.A.8** after generating the traffic ticket the data is not discarded but remains stored in order to provides statistics about it.

The third state diagram represents the generation of suggestions for possible interventions in unsafe areas/streets by means of SafeStreets, suggestions then showed in the Authority interface.
In the cross data state, obviously, in case of unmatching data nothing is done and showed to the Users.
![](./images/state_diagram_3.png)

figura 4: Suggestion Generation

---


The last state diagram represents the show of statistics about traffic violations, is the more complex due to the different visibility that we must offer to users depending on whether they are authorities users or just users.
![](./images/state_diagram_4.png)

figura 5: Statistics Generation

---

### 2.2 Product Functions
According to the IEEE [2] this paragraph contain the summary of the product functions.

- Traffic violation report: this is the main function and even the basic service provided by our S2B. The system allow a user to sign up and entering his credentials, the system requires also the ID number to prevent multiple and fake sign up. A logged user after the sign in is able to notify the authorities about a traffic violation by inserting the mandatory information related to it like: location (exploit GPS and Google MAP APIs as mentioned before [2.1]), type of the violation, date and time and a picture of the violation (with the license plate easily visible in the picture),after that the system retrieve the data and runs an external algorithm (OCR) to read the license plate from the picture, then show in the user interface what was read and the user can confirm or can modify it
- Suggest Interventions
- Traffic Ticket Generation

### 2.3 User characteristics
User can use the system to report violations of the traffic laws when these occur, the conditions that must be guaranteed from the user to use the application are: 

- He must have a smartphone and be able to use it
- The smartphone must have camera and GPS (forse questa è una assunzione di dominio più che una caratteristica dell utente)

## 2.4 Domain assumption, dependencies and constraints
We assume that these assumptions hold true in the domain of our system

- **D.A.1**: the positions retrived by the GPS of the user's phone is accurate (5m± of tollerance)
- **D.A.2** The information about a reported traffic violation inserted by an user are correctly encoded in the system
- **D.A.3** SafeStreets contains all the possible type of a traffic violations (non esistono violazioni del codice della strada che non possano essere inviate a safestreets)
- **D.A.4** The algorithm used to recognize the license plate from a picture is an external library with relative APIs 
- **D.A.5** The traffic violation provided by the user are real infringements of the traffic laws
- **D.A.6** A policeman search for the traffic violation that belong to his municipality before generating a traffic ticket, in fact, it will generate a traffic ticket only if the traffic violation belongs to his jurisdiction ignoring the others.
- **D.A.7** After generating a traffic ticket the traffic violation is not discarded by our system in order to provide statistics about traffic violations, but it is no more showed in the authority interface.
- **D.A.8** The authority users are provided by the developer in order to ensure the identity of the authority.
- **D.A.9** The picture inserted by the user must contain the license plate of the vehicle
- **D.A.10** The picture provided by a user during the notify service shows the infrangement or infrangements committed by the vehicle
- **D.A.11** The maps provided by the Google Maps APIs are always up to date
- **D.A.12** The position of the users is always available when it interfaces with SafeStreets

