# osTicket Help Desk Lab

## Project Overview

Self-hosted **Debian Server** and configured **osTicket** help desk environment, utilizing the **LAMP** techstack. (Linux, Apache, MariaDB, PHP). Purpose was to create, from available resources, a working support system that handles ticket requests, familiarize myself with standard **ITIL incident-management workflow**, and troubleshooting any network/technical issues. 

## Objectives

- Setup a local server to host help desk ticketing system
- Simulate a work environment that categorizes tickets, priority levels, departments, and creating agents
- Document installation, configuration, and troubleshooting issues

## Technologies Used 

- **OS:** Debian Server (VM)
- **Stack:** Apache2 - MariaDB - PHP 8.1
- **App:** osTicket v1.17.x 
- **Access:** LAN for staff/customer portals 

## Technical Work 

- **Successful deployment of osTicket on self hosted server** Used Proxmox to create a LXC that runs Debian 
- **Departments** Support, Maintenance, Sales; T1, T2, T3 teams 
- **Help Topics** Hardware, Software, Connectivity, Password Reset, Ticket Management
- **Agents** Created agents with specific permissions and job levels

## Screenshot

### **Landing Page** ---- Allows users to submit and track their tickets 

<img width="843" height="539" alt="Image" src="https://github.com/user-attachments/assets/5981c773-0d50-42d6-9be3-60a623db1563" />

### **Agent Panel** ---- Tickets-queue that agent's have access to 

<img width="962" height="394" alt="Image" src="https://github.com/user-attachments/assets/053a6790-40be-4f25-95ca-040eb62f837c" />

### **Admin Panel** ---- Management Panel for overview of tickets 

<img width="962" height="416" alt="Image" src="https://github.com/user-attachments/assets/40fab5b1-8ae4-4ee5-af2a-b8a50f390178" />

### **Ticket Management** ---- Management can escalate or reassign tickets

<img width="956" height="961" alt="Image" src="https://github.com/user-attachments/assets/08d864f5-dc9e-4ad3-8dcd-1429ddb46648" />


## Experience Gained 

- Installing a **LAMP** application onto Linux
- TCP/IP Networking and Connectivity troubleshooting workflow 
- Testing how devices communicate with one another over a LAN
- Importance of categorizing prioritization to organize work environment and quickly aid users 

## Future Plans

- Convert static server into dynamic to allow remote access
- Buff up security firewalls
- Create an FAQ of issues to minimize repeat tickets
