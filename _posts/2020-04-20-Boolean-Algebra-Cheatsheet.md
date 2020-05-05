---
layout: post
title: Boolean Algebra Cheat Sheet 
category: Computer Science
tags: [Computer Science, Math]
excerpt_separator: <!--more-->
---

I previously posted a [logic rules cheat sheet]({% post_url 2020-02-02-Logic-Rules-Cheat-Sheet %}) and figured it was about time that I do the same for boolean algebra. 

<!--more-->

| Expression        | Equivalent To        | Name of the Rule        |
|:-----------------:|:--------------------:|:-----------------------:|
| $$ X + Y $$       | $$ Y + X $$          | Commutative             |
| $$ X \cdot Y $$   | $$ Y \cdot X $$      | Commutative             |
| $$ (X + Y) + Z $$ | $$ X + (Y + Z) $$    | Associative             |
| $$ (X \cdot Y) \cdot Z $$ | $$ X \cdot (y \cdot Z) $$ | Associative  |
| $$ X + (Y \cdot Z) $$ | $$ (X + Y) \cdot (Z + Z) $$ | Distributive |
| $$ X \cdot (Y + Z) $$ | $$ (X \cdot Y) + (X \cdot Z) $$ | Distributive |
| $$ X + 0 $$       | $$ X $$              | Identity                |
| $$ X \cdot 1 $$   | $$ X $$              | Identity                |
| $$ X + X' $$      | $$ 1 $$              | Complement              |
| $$ X \cdot X' $$  | $$ 0 $$              | Complement              |
| $$ X + X $$       | $$ X $$              | Idempotence             |
| $$ X \cdot X $$   | $$ X $$              | Idempotence             |