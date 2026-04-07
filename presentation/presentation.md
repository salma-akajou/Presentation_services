---
marp: true
theme: default
_class: lead
_paginate: false
paginate: true
backgroundColor: #ffffff
style: |
  section {
    font-size: 22px;
    color: #1a1a1a;
    line-height: 1.6;
    padding: 60px 80px;
  }

  footer { 
    width: 100%; 
    text-align: right; 
    font-size: 14px; 
    color: #0B3C5D; 
  }

  .logo-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: absolute;
    top: 40px;   
    left: 60px;
    right: 60px;
  }

  .logo-header img { 
    height: 140px; 
    margin-left:10px; 
    margin-right:10px 
  }

  h1 { 
    color: #0B3C5D; 
    font-size: 2.8em; 
    margin-top: 100px; 
    text-align: left; 
  }

  h2 { 
    color: #0B3C5D; 
    font-size: 2em; 
    border-bottom: 3px solid #0B3C5D; 
    margin-bottom: 40px;
  }

  h3 { 
    text-align: left; 
    color: #123; 
    margin-top: 0; 
  }

  .sommaire-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    margin-top: 20px;
  }

  .sommaire-item {
    display: flex;
    align-items: center;
    background: #eaf2f8;
    border-radius: 12px;
    padding: 15px 20px;
    border-left: 6px solid #0B3C5D;
  }

  .sommaire-num {
    background: #0B3C5D; 
    color: white; 
    width: 35px; 
    height: 35px;
    display: flex; 
    justify-content: center; 
    align-items: center;
    border-radius: 50%; 
    font-weight: bold; 
    margin-right: 15px; 
    flex-shrink: 0;
  }

  .img-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100%;
  }

  .img-methodo {
    width: 85%;
    height: auto;
    max-height: 450px;
    object-fit: contain;
    border-radius: 10px;
    box-shadow: 0 10px 25px rgba(11,60,93,0.2);
  }

  .dt-card {
    background: #f4f9fc;
    padding: 30px;
    border-radius: 12px;
    border-top: 6px solid #0B3C5D;
    text-align: left;
    margin-top: 20px;
    width: 100%;
    box-shadow: 0 5px 15px rgba(0,0,0,0.05);
  }

  .tech-container {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-top: 20px;
  }

  .badge-simple {
    padding: 8px 18px;
    border-radius: 6px;
    font-weight: 600;
    background-color: #0B3C5D;
    color: #ffffff !important;
    font-size: 0.85em;
    border: none;
  }

  .maquette-grid {
    display: flex;
    gap: 15px;
    justify-content: center;
    align-items: flex-start;
    height: 350px;
  }
---

<div class="logo-header">
  <img src="images/ofppt-logo.png" alt="Logo Left">
  <img src="images/logo-solicode.png" alt="Logo Right">
</div>

# Validation des Services et Données

### Factories & Tests

**Réalisé par :** Salma Akajou
**Encadré par :** M. ESSARRAJ Fouad

---

## 🗂️ Sommaire

<div class="sommaire-grid">
  <div class="sommaire-item"><div class="sommaire-num">1</div>Introduction</div>
  <div class="sommaire-item"><div class="sommaire-num">2</div>c'est quoi Factories ?</div>
  <div class="sommaire-item"><div class="sommaire-num">3</div>à quoi sert les services ?</div>
  <div class="sommaire-item"><div class="sommaire-num">4</div>Pourquoi tester les services ?</div>
  <div class="sommaire-item"><div class="sommaire-num">5</div>Exemple</div>
  <div class="sommaire-item"><div class="sommaire-num">6</div>Bonne Pratique </div>
</div>

---

## 🎯 Introduction

<div class="dt-card">
Dans une application, la logique importante se trouve dans les services.

 Pour garantir leur fiabilité, on utilise des tests.
 Ces tests ont besoin de données → d'où l'utilisation des Factories.

</div>

---

## 🧩 Les Factories

<div class="dt-card">
Une Factory permet de générer automatiquement des données.

✔️ Elle évite la création manuelle
✔️ Elle accélère les tests
✔️ Elle produit des données réalistes

 Elle est essentielle pour préparer l'environnement de test.

</div>

---

## ⚙️ Les Services

<div class="dt-card">
Les Services contiennent la logique métier.

 Exemples :

* rechercher des données
* appliquer une règle métier
* traiter une action

 Ils sont indépendants du contrôleur.

</div>

---

##  Les Tests

<div class="dt-card">
Les tests permettent de vérifier :

* que le service fonctionne correctement
* que les résultats sont fiables
* que les erreurs sont gérées

👉 Objectif : éviter les bugs.

</div>

---

## 🔄 Les scénarios à couvrir

<div class="dt-card">
<strong>Cas de succès :</strong>
- données valides
- résultat attendu

<strong>Cas d’échec :</strong>

* données incorrectes
* résultat vide
* erreur

 Un bon test couvre les deux.

</div>

---

##  Exemple Factory

```php
User::factory()->count(3)->create(['status' => 'inactive']);
```

Permet de générer plusieurs cas facilement

---

## ⚙️ Exemple Service

```php
class UserService
{
    public function activateUser($email)
    {
        $user = User::where('email', $email)->first();

        if (!$user) {
            return null;
        }

        if ($user->status === 'active') {
            return $user;
        }

        $user->status = 'active';
        $user->save();

        return $user;
    }
}
```
Logique : Chercher user, Vérifier s'il existe, Vérifier son état, Le modifier si nécessaire

---

## ✅ Test succès

```php
public function test_activate_user_success()
{
    $user = User::factory()->create([
        'email' => 'test@test.com',
        'status' => 'inactive'
    ]);

    $service = new UserService();

    $result = $service->activateUser('test@test.com');

    $this->assertEquals('active', $result->status);
}
```
 Vérifie que le user passe de inactive → active

---

## ❌ Test échec

```php
public function test_activate_user_not_found()
{
    $service = new UserService();

    $result = $service->activateUser('no@test.com');

    $this->assertNull($result);
}
```

---

## 🚀 Bonnes pratiques

<div class="dt-card">

- Utiliser toujours Factory
- Tester succès et échec
- Tests simples et clairs

</div>

---

## 🧾 Conclusion

<div class="dt-card">
Les Factories simplifient la création des données.

Les Services doivent être testés pour garantir la qualité.

 Tester tous les cas = application fiable.

</div>

---
<div class="dt-card">
Merci pour votre attention 🚀
</div>