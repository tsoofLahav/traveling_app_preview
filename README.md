# traveling_app_preview

**Note:** This repository is for preview purposes only. The app is the proprietary property of *The Betesh Group*.

## Description

This is a travel app currently in development. It is designed with a structured interface that allows non-technical staff at *The Betesh Group* to easily upload images and data about various locations directly within the app.  
The project was built from scratch as part of an internship and serves as the foundation for the full application.

## Demo

[Watch a short demo](https://www.youtube.com/shorts/z2f5DMXWMnA)

## Architecture

**Frontend Structure:**  
The app follows a hierarchical (tree-like) menu design. Each page (containing images and metadata) is saved in the database using a list of keys representing its path within the tree.

<img width="1132" height="638" alt="Screenshot 1" src="https://github.com/user-attachments/assets/523b18ad-8606-4ea7-aaa1-d9b94346cdd8" />

**Backend Structure:**  
The backend efficiently retrieves high-quality images from a large image dataset based on structured queries to the database.

<img width="1151" height="647" alt="Screenshot 2" src="https://github.com/user-attachments/assets/a4b003ac-639d-4f4b-b739-c891cd3a41c9" />

## Technology Stack

- **Frontend:** Flutter, Dart  
- **Backend:** Flask, Python  
- **Database:** PostgreSQL  
- **Hosting:** Azure (App Service + Database)
