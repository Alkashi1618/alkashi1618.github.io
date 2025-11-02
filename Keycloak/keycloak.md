# **Mise en place d’un système d’authentification centralisée avec Keycloak**

## Introduction

Dans le cadre de la sécurisation et de la centralisation de l’accès aux différentes applications internes, l’entreprise souhaite mettre en place une solution d’authentification unifiée basée sur Keycloak. Ce TP vise à comprendre le fonctionnement d’un système SSO (Single Sign-On) avec OAuth2 et OpenID Connect.

## Objectif

Mettre en place un système d’authentification centralisé permettant aux différentes applications web de l’entreprise de déléguer l’authentification à un serveur Keycloak. L’objectif est de comprendre les rôles OAuth2, les flux de connexion sécurisés (Authorization Code + PKCE) et la gestion des utilisateurs, rôles et clients.

## Partie 1 – Installation de Keycloak

Commande d’installation via Docker :

<aside>

docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:24.0.3 start-dev

</aside>

## Partie 2 – Configuration de Keycloak

Étape 1 : Création du Realm ‘tdsi’

Étape 2 : Création des utilisateurs Alice et Bob

Étape 3 : Attribution des rôles ‘user’ et ‘admin’

## Partie 3 – Création et configuration des clients

Deux clients sont créés : un client backend (Confidential) et un client mobile (Public) utilisant PKCE.

- backend-api : Type Confidential, Service Accounts Enabled = ON

- mobile-app : Type Public, Standard Flow + PKCE activé


## Partie 4 – Test de l’authentification

1. 1. Lancer l’API Node.js sécurisée par Keycloak :

1. 2. Tester avec Postman (Authorization Code + PKCE) :

1. 3. Appeler l’API protégée :

## Partie 5 – Analyse et compréhension

## 1. Différence entre OAuth2 et OpenID Connect

- **OAuth2** est un protocole d’autorisation permettant à une application d’accéder à des ressources sans transmettre le mot de passe utilisateur, via un access token (jeton d’accès).
- **OpenID Connect** (OIDC) ajoute une couche d’authentification sur OAuth2. Il émet un id_token (souvent un JWT) contenant des informations sur l’identité de l’utilisateur, permettant à l’application de s’assurer de qui est connecté, en plus de contrôler les accès.

## 2. Que contient un JWT

Un JWT (JSON Web Token) est formé de trois parties :

- **Header** : Spécifie l’algorithme de signature et le type du jeton.
- **Payload** : Contient les claims, c’est-à-dire les données sur l’utilisateur, ses autorisations, ses rôles, etc.
- **Signature** : Permet de valider l’intégrité du jeton (qu’il n’a pas été modifié).

## 3. Comment Keycloak vérifie les permissions

Keycloak attribue à chaque utilisateur authentifié un JWT (access_token) contenant ses rôles et permissions, correspondant à ce qui est défini dans le realm et au niveau des clients.

Les applications ou APIs consultent ce token pour déterminer si l’utilisateur possède les droits suffisants pour accéder à une ressource ou fonctionnalité précise, en se basant sur les rôles et claims du token. La vérification se fait donc via décodage et inspection du contenu du JWT, qui sert de preuve d’identité et d’autorisation.

## **Résumé:**

## 1️⃣ Keycloak – C’est quoi ?

Keycloak est une solution open-source

de gestion des identités et des accès (Identity and Access Management – IAM﻿).
 Il permet de centraliser l’authentification et l’autorisation des 
utilisateurs pour diverses applications web, API ou mobiles, et offre un
 système de Single Sign-On (SSO)

.

## Fonctionnalités principales :

- **Authentification centralisée** : les utilisateurs se connectent une seule fois pour accéder à plusieurs applications.
- **Gestion des utilisateurs** : création, modification, suppression d’utilisateurs, gestion des rôles et groupes.
- **Protocoles standards supportés** : OAuth2, OpenID Connect (OIDC)﻿, SAML 2.0
- **Gestion des rôles et permissions** : contrôle fin d’accès aux ressources.
- **Sécurité renforcée** : support de 2FA, WebAuthn﻿ (biométrie), OTP, etc.
- **Identité fédérée (Identity Brokering)**: possibilité de se connecter via Google, Facebook, AzureAD, ou d’autres serveurs Keycloak.
- **Jetons et tokens** : utilisation de JWT (JSON Web Token) pour sécuriser les échanges entre clients et serveurs.

En résumé, Keycloak agit comme un serveur d’autorisation et d’authentification. Il permet de déléguer la gestion des utilisateurs et de leurs droits à un serveur central, plutôt que de gérer chaque application individuellement.

---

## 2️⃣ Comment l’utiliser ?

## Étapes principales :

- **Installation** Docker
    
    `bashdocker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:24.0.3 start-dev`
    
- **Accès** : [http://localhost:8080](http://localhost:8080/)
- **Création d’un Realm**
    
    Un realm est un espace isolé pour gérer utilisateurs, rôles et applications. Exemple : tdsi.
    
- **Gestion des utilisateurs**
    
    Création des comptes (alice, bob) et définition des mots de passe.
    
    Configuration des actions requises (Required User Actions) : OTP﻿, WebAuthn, vérification email, etc.
    
- **Création des rôles**
    
    Exemple : user, admin.
    
    Affectation de ces rôles aux utilisateurs pour gérer l’accès aux applications.
    
- **Création de clients (applications)**
- Client public : mobile ou SPA, ne peut pas garder de secret.
- Client confidentiel : backend ou API pouvant stocker un client_secret.
- **Configuration des flows OAuth2** : Authorization Code, PKCE﻿, Client Credentials, etc.
- **Test et intégration**
    
    Applications peuvent être en PHP, Node.js, Spring Boot. Vérification du flow d’authentification, obtention des tokens JWT, validation côté serveur.
    

---

## 3️⃣ Pourquoi l’utiliser ?

- **Centralisation de l’authentification**
    
    Évite de créer un système d’authentification différent pour chaque application.
    
- **Sécurité renforcée**
    
    Mot de passe jamais partagé avec les applications clientes.
    
    Support de 2FA, WebAuthn﻿, OTP.
    
- **Contrôle fin des permissions**
    
    Rôles et groupes permettent de restreindre l’accès à certaines ressources.
    
- **Interopérabilité**
    
    Supporte OAuth2, OIDC﻿ et SAML, facilitant l’intégration avec d’autres systèmes.
    
- **Expérience utilisateur améliorée**
    
    SSO: l’utilisateur se connecte une seule fois pour accéder à toutes les applications.
    
- **Scalabilité**
    
    Idéal pour les entreprises avec plusieurs applications internes ou microservices.
    

---

## 4️⃣ Où l’utiliser ?

- Entreprises ayant plusieurs applications internes ou externes.
- Projets nécessitant authentification unique (SSO) pour améliorer la sécurité et l’expérience utilisateur.
- Applications mobiles ou web avec API sécurisées.
- Scénarios où des tiers doivent accéder à des ressources sans partager leurs identifiants.
- Organisations nécessitant rôles, permissions et audit centralisés.

---

## Comment utiliser Keycloak dans votre infrastructure et avec vos applications ?

Pour intégrer Keycloak et sécuriser vos applications, les étapes clés sont :

- **Déploiement de Keycloak**
    
    Installer Keycloak sur un serveur ou via un conteneur Docker, pour disposer d’une instance accessible par votre infrastructure.
    
- **Création d’un Realm**
    
    Configurer un realm, espace isolé pour gérer les utilisateurs, rôles et applications. Chaque realm représente un périmètre de sécurité distinct.
    
- **Définition des utilisateurs et rôles**
    
    Créer les comptes utilisateurs, définir leurs rôles et permissions, et configurer les actions requises (comme l’activation de la double authentification).
    
- **Enregistrement des applications en tant que clients Keycloak**
    
    Chaque application (web, mobile, API) est enregistrée dans Keycloak comme un client. Deux types :
    
    - *Client public* : applications front-end (SPA, mobile) sans secret côté client.
    - *Client confidentiel* : backend, API capables de sécuriser un secret client.
- **Configuration des protocoles d’authentification**
    
    Utiliser les flows OAuth2 et OpenID Connect adaptés (Authorization Code, PKCE, Client Credentials) pour permettre aux applications d’obtenir des jetons d’accès (access_token) et d’identité (id_token).
    
- **Intégration dans les applications**
    
    Les applications utilisent les bibliothèques clientes Keycloak (ou standards OAuth2/OIDC) pour rediriger l’utilisateur vers le serveur d’authentification Keycloak, recevoir les tokens JWT, et valider l’identité et les droits.
    
- **Validation côté serveur**
    
    Les APIs et serveurs valident les tokens JWT reçus, vérifient la signature et les rôles d’accès avant d’autoriser les requêtes.
    
- **Gestion centrale et audits**
    
    Grâce à Keycloak, la gestion des accès est centralisée, facilitant la mise à jour des droits, le suivi des connexions, et la conformité.
    

---

Cette approche montre comment Keycloak agit comme un serveur d’identité et d’autorisation centralisé, interfacé avec des applications via des protocoles standards, simplifiant la sécurité et l’administration de votre système informatique.

## Conclusion

Ce mini LAB nous a permis de comprendre la mise en place d’un serveur d’identité Keycloak et l’intégration d’un système d’authentification centralisé basé sur les standards OAuth2 et OpenID Connect. La configuration des rôles, utilisateurs, clients et des flux PKCE permet d’assurer un haut niveau de sécurité tout en simplifiant la gestion des accès.
