# TP SOAP Bank - Réponses

## B) Lecture du contrat

### 1. Fichier XSD et son rôle

**Fichier XSD :** `src/main/resources/bank.xsd`

**Rôle :** Le fichier XSD (XML Schema Definition) définit le contrat du service SOAP selon l'approche **contract-first**. Il spécifie :
- La structure des messages XML échangés
- Les types de données autorisés
- Les éléments de requête et réponse
- Le namespace du service (`http://example.com/bank`)

Le fichier XSD génère automatiquement les classes Java correspondantes via le plugin JAXB Maven.

### 2. Éléments de requête/réponse

#### GetAccount
- **Requête :** `GetAccountRequest`
  - `accountId` (string) : Identifiant du compte

- **Réponse :** `GetAccountResponse`  
  - `account` (AccountType) : Informations complètes du compte

#### Deposit  
- **Requête :** `DepositRequest`
  - `accountId` (string) : Identifiant du compte
  - `amount` (decimal) : Montant à déposer

- **Réponse :** `DepositResponse`
  - `newBalance` (decimal) : Nouveau solde après dépôt

#### Type complexe AccountType
- `accountId` (string) : Identifiant du compte
- `owner` (string) : Propriétaire du compte  
- `balance` (decimal) : Solde du compte
- `currency` (string) : Devise du compte

### 3. Analyse du WSDL

**URL du WSDL :** http://localhost:8080/ws/bank.wsdl

**Namespace :** `http://example.com/bank`

**PortType :** `BankPortType` 
- Opérations définies :
  - `GetAccount` : Consultation des informations d'un compte
  - `Deposit` : Dépôt d'un montant sur un compte

**Endpoint (URL) :** `http://localhost:8080/ws`

**Binding SOAP :** SOAP 1.1 avec transport HTTP

## C) Tests Postman réalisés

### GetAccount - Cas nominal (A100)
✅ **Statut :** Succès  
**Compte testé :** A100 (Alice)  
**Résultat :** Informations du compte retournées correctement

### Deposit - Cas nominal (A100, 20.00 TND)  
✅ **Statut :** Succès  
**Nouveau solde :** 170.00 TND (150.00 + 20.00)

### SOAP Fault - Cas d'erreur
✅ **Erreurs testées :**
- Montant négatif (-10.00) → InvalidAmountException
- Compte inexistant (X999) → UnknownAccountException

## D) Fonctionnalité ajoutée

### Opération choisie : **Withdraw** (Retrait)

**Description :** Permet de retirer un montant d'un compte bancaire avec validation du solde suffisant.

### Fichiers modifiés :

1. **XSD étendu :** `src/main/resources/bank.xsd`
   - Ajout de `WithdrawRequest` et `WithdrawResponse`
   
2. **Endpoint :** `src/main/java/com/example/bank/endpoint/BankEndpoint.java`  
   - Nouvelle méthode `withdraw()`
   
3. **Service :** `src/main/java/com/example/bank/service/BankService.java`
   - Logique métier pour le retrait
   - Nouvelle exception `InsufficientFundsException`

### Tests Postman Withdraw :

#### Cas nominal (A100, retrait 50.00)
✅ **Résultat :** Nouveau solde = 100.00 TND

#### Cas d'erreur - Solde insuffisant  
✅ **SOAP Fault :** InsufficientFundsException

#### Cas d'erreur - Compte inexistant
✅ **SOAP Fault :** UnknownAccountException