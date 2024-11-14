# Dynamics 365 Revamp 

During my role as a Discovery Intern at Microsoft, I engaged in an enriching learning journey focused on advancing my technical prowess in Python and mastering the collaborative tools within Azure DevOps. Working closely with the Customer Experience & Success team, I tackled challenges aimed at enhancing support infrastructure. Throughout the internship, I played a pivotal role in developing a solution to improve the efficiency of communication among Support Engineers using Dynamics 365. By integrating new features, our solution streamlined communication processes, fostering a more productive workflow. This experience deepened my technical skills while also honing my ability to solve customer-centric challenges within a dynamic team environment.

## Project Overview: 

How can engineers maintain a first quality resolution with customers effectively managing customer expectations while navigating different tools to resolve the customer's issue quickly?

DfM is a case management solution that assists Support Engineers and Advocates the ability to assist Microsoft's commercial and consumer customer.

### Pros and Cons
| Pros                                             | Cons                                     |
|--------------------------------------------------|------------------------------------------|
| Provides contact info for all necessary contacts | Separate application for communication   |
| Able to send emails from portal                  | Fixed layout                             |
| Widely used by most support engineers            | Only access to one’s email or phone number |


### 👥 **Users**  
Microsoft support engineers using **MD365**

---

### ❓ **Problem**  
Communication between support engineers and customers is inefficient.

---

### 💡 **Solution**  
Embed a **Teams chat box** within the MD365 workspace to allow faster communication and ensure problems are solved efficiently.


## Key Considerations: 
- Re-vamp of User Interface for Dynamics for Microsoft Tool
- Integrated Chatbot

## User Story: 
*"As a business customer I want to know who has access to my case information so, I can maintain privacy and confidentially of my personal information."*

## Key Takeways:
- One-stop shop for all things communication
- Signficantly reduce SLA first resolution time with customer and collaboration time between interal engineers
- A customized dashboard for engineers to work on cases and collaboration

## Microsoft Dynamics Support Enhancement EPIC 🚀

### Summary
We intend to revise the Microsoft Dynamics support engineer processes to promote more efficient collaboration and enhanced workflow.

### Introduction
Engineers will have an easier time communicating with each other and customers. This will reduce support times and improve both the customer and employee experience.

## Product Requirements

#### Support Engineer Features Access 🔧
Support Engineers have lots of features at their fingertips through Microsoft Dynamics. Making these features more accessible is an important part of this Epic. Additionally, the rework will include a chatbox integration powered by the Teams Chat API.

### Design & Technical Requirements 💻
Support Engineers should have an easier time learning the Dynamics platform and be able to modify it to their taste.

## Features

### Feature 1: Embedded Teams Chat

#### Business Hypothesis
(Embedding) We need to implement an embedded Teams Chat in order to allow ease of access for setting up meeting between customer and escalated engineer by the first point of contact engineer.

#### Business Value
- Currently we plan on only engineers that have access to Dynamics for Microsoft only having access to this feature
- We expect engineers only to utilize this feature when there could be requests an escalation request or a collaboration meeting with another engineer
- The urgency of this feature is particularly high because this could possibly convert a 2-3 day process to a 1-2 day process

### Feature 2: Previous Case Finder

#### Business Hypothesis
Finding previously solved case by engineer: By listing engineers previously completed cases, engineers given an escalation collaborate given an extra set of data. This will allow engineers who are looking to collaborate to have a more informed decision in terms to who they should reach out to.

#### Business Value
- Currently we plan on only engineers that have access to Dynamics for Microsoft having access to this feature
- We expect engineers only to utilize this feature when there could be requests an escalation request or a collaboration needs with another engineer
- The urgency of this feature is high because it has been flagged (in a job title) that acts to connect requester to who can provide support

### Description
Integrated Teams-esque chat: teams chat but brought into the contact portion of dynamics for Microsoft.

### Acceptance Criteria
- Given that I am a customer support engineer, I want a platform where I can quickly add engineers who I am seeking to collaborate with
- The platform needs to have profiles and also communication via Teams chat
- A link to an engineer's previously solved cases in their profile for Teams chat (only will show up in embedded feature version of Teams chat to clarify)

---
*Note: This document outlines the enhancement of Microsoft Dynamics support processes through improved collaboration features and workflow optimization.*

## Early Development:
(No pictures for company confidently)
- Dynamics 365 had a fixed layout
- was not customizable for personal preferences 
- only presented intromation pertaining to the problem, no details like contact info or additional commentary to solve the presented problem
- Support engineers had to manually contact the customers
- There was a lack of ease in acess

## Post Development 
### 💬 **TEAMS Chat Features**

- **All TEAMS chat features are implemented** with **additional enhancements** 🛠️
  
---

### 🗂️ **Additional Information Access**
- **View more** than just email & phone details 📧📱

---

### 🎨 **Customizable Interface**
- **Personalize** the interface to match your preferences 🎛️
