---
date: 2026-10-07
tags:
  - Practices
  - Security
  - Naming
  - Git
description: "Do's and don'ts for developers: security, naming, publishing and commits"
---
## 📋 **Table of Contents**
1. [[#Security]]
2. [[#Naming]]
3. [[#Publishing an app]]
4. [[#Commits]]

---

## Security

- Don't leave your email address in plain text in a `mailto:` link. Use an HTML anchor instead.

## Naming

`checkPassword` is a wrong name for this function:

```js
function checkPassword(password){
	if (password === storedPassword){
		Session.initialize();
		return true;
	}
	return false;
}
```

Someone may use it only to check a password (for example a new password and its confirmation) and, without wanting or knowing it, initialise the user's session.

Rename it `checkPasswordAndInitializeSession`, or better, create two functions:

```js
if (checkPassword(password)){
	initializeSession();
}
```

## Publishing an app

- Add Google login and Apple login directly: it gives a better sign-up rate.
- Remove every barrier between your user and your app.

## Commits

- Keep clean commits: squash them.
- Avoid having too many commits.

---
## 📚 Ressources
- [Example: naming a function](https://www.instagram.com/p/DUIoSnnDJiH/)
