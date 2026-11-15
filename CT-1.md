<!--
# Student: **Joy Sarker**
# Student ID: IT23022
Project: Foundations of Number Theory and Public-Key Cryptography
-->

<h1 align="center">Foundations of Number Theory & Public-Key Cryptography</h1>

<h2 align="center">
  <b>Submitted By:&nbsp;&nbsp;</b> Joy Sarker &nbsp; | &nbsp;
  <b>Student ID:&nbsp;&nbsp;</b> IT23022
</h2>

<p align="center">
  <i>An analytical and computational exploration of integer properties, modular systems, and classical RSA encryption implemented in Python.</i>
</p>

---

## Contents Overview

1. [Overview and Motivation](#1-overview-and-motivation)
2. [Core Objectives](#2-core-objectives)
3. [Computational Setup](#3-computational-setup)
4. [Elementary Integer Properties](#4-elementary-integer-properties)
5. [Primes and Prime Decomposition](#5-primes-and-prime-decomposition)
6. [GCD and the Euclidean Method](#6-gcd-and-the-euclidean-method)
7. [Arithmetic in Modular Systems](#7-arithmetic-in-modular-systems)
8. [Linear Congruences and Multiplicative Inverses](#8-linear-congruences-and-multiplicative-inverses)
9. [Fermat’s and Euler’s Classical Theorems](#9-fermats-and-eulers-classical-theorems)
10. [The Chinese Remainder Theorem (CRT)](#10-the-chinese-remainder-theorem-crt)
11. [Principles of Modern Cryptography](#11-principles-of-modern-cryptography)
12. [The RSA Cryptosystem](#12-the-rsa-cryptosystem)
13. [Interactive Python Lab Experiments](#13-interactive-python-lab-experiments)
14. [Step-by-Step Solved Problems](#14-step-by-step-solved-problems)
15. [Practical Security Considerations](#15-practical-security-considerations)
16. [Summary of Acquired Skills](#16-summary-of-acquired-skills)

---

## 1. Overview and Motivation

Number theory focuses on the structure and behavior of whole numbers. Topics like divisibility rules, prime distribution, common divisors, and remainder arithmetic form the theoretical backbone of this field. While historically viewed as pure mathematics, these concepts now power the security infrastructure of the modern internet.

Cryptography applies mathematical hardness to protect digital data—ensuring privacy, data integrity, and sender verification. Unlike traditional secret-key methods, asymmetric (public-key) cryptography relies on a mathematically linked key pair: one key shared openly for encryption, and a matching secret key kept private for decryption.

This report bridges theory and implementation by translating foundational number-theoretic theorems into clean Python scripts supported by verifiable numerical tests.

> **Reference Material:** Adapted from the [Number Theory and Cryptography Workshop — mnp-club](https://github.com/mnp-club/Number_Theory_and_Cryptography_Workshop).
>
> *Note: This documentation serves as an independent study compilation. Course-specific tasks should be cross-referenced with official class materials.*

---

## 2. Core Objectives

Upon reviewing and executing the code modules in this study, one will be able to:

- Define integer divisibility, primality, unique factorization, and modular equivalence.
- Code both the standard and extended versions of the Euclidean Algorithm from scratch.
- Compute fast modular exponentiation and find modular multiplicative inverses.
- Apply Fermat's Little Theorem, Euler's Totient Theorem, and the Chinese Remainder Theorem to solve congruence systems.
- Contrast symmetric encryption with asymmetric public-key architectures.
- Execute a complete end-to-end RSA key generation, encryption, and decryption cycle in Python.

---

## 3. Computational Setup

All scripts are written in standard Python 3 and require no external cryptographic packages. They can be executed locally via Jupyter or directly in the cloud using [Google Colab](https://colab.research.google.com/).

**Local Environment Setup:**

```bash
python3 --version
pip install notebook matplotlib
jupyter notebook
