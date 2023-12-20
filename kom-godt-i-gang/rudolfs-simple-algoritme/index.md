+++
title = 'Rudolfs Simple Algoritme'
categories = ['Kom godt i gang']
date = 2023-12-01T10:10:50+01:00
+++

## Challenge Name:

Rudolfs Simple Algoritme

## Category:

Kom godt i gang

## Challenge Description:

Rudolf har krypteret flaget med RSA, men han har glemt, hvilke værdier han må dele, og hvilke der er hemmelige.

Kan du dekryptere hans flag?

```bash
n = 18925541077824988712183209199079650116566712142533263598140310312267869339909616217074219588372785445276555780123046115230350459565816223330383386247738973670953655501222133239338895736270254698555339805998318917910649931316023627438828262249797009825103440676597924656257968596106423540073674857036082985165252462511218699089244945467187571072623490579151099801396985689250933808835629428463352657696286415936289158629585299425218570781030764634615060453765139004555499560932741462331656722996410976468325012470545433354833815198728434027509389192307483003046507871036717299333210733499795748579118306095556829263639
q = 118063811052597078076648057208258290925722946222241910613035885203409338850020920906592375946233241094333077964833028928470213309573854399800445879752747335760515740197890395360028081095608749779850729355973826460560998420377412739941103494034901004876773916365447039754161974845578315476777413979436560683093
e = 65537
ct = 15436002982413531167792439197855366628511742032858020132381247448034583641589139053141197248441750465498638214768245135475992457602527123482967943141258327043927292925664644100351630294623058653796001739830096433745160075993427695839812899234549970407943516319088414287369653326567748224801324184732924590389759109676037235181214031030181051149964539917324617030312886744245699648952675273750261984402368712095508858334065429006024222968042848231102366353871199812809955626892045929470339878801763957059048596399598780789796091488847291534189965452758394821078560041332079868199891048764803339019365629760271406619695
```

_Se f.eks. [her](https://www.au.dk/fileadmin/www.au.dk/subuniversity/datalogi.pdf) for mere info om, hvordan RSA fungerer_

## Approach

In this task, we are given a set of RSA parameters and an encrypted flag:

In RSA, two large prime numbers p and q are chosen, and from them, n = p \* q is calculated, which is used as the modulus. An exponent e is chosen, and the public key for encryption then consists of e and n.

The private key d is calculated as the inverse of e modulo phi(n), that is, d = e^-1 mod phi(n). For the calculation, phi(n) is simply phi(n) = (p - 1) \* (q - 1), when n is a product of two different prime numbers.

The security of RSA lies in the fact that only the recipient knows the two prime numbers p and q because with them phi(n) and thus d can be quickly found. Therefore, an important aspect of security is that it is difficult (practically impossible for large enough prime numbers) to factorize n and find p and q from it.

However, Rudolf has been a bit confused and given us one of the prime numbers, q. With it, we can trivially find p and from there the private key, which can be used to decrypt the flag:

The following Python script can be used to solve the challenge

```python
from sympy import mod_inverse

## Implementation based on following documentation (in Danish) https://www.au.dk/fileadmin/www.au.dk/subuniversity/datalogi.pdf
# Given values
n = 18925541077824988712183209199079650116566712142533263598140310312267869339909616217074219588372785445276555780123046115230350459565816223330383386247738973670953655501222133239338895736270254698555339805998318917910649931316023627438828262249797009825103440676597924656257968596106423540073674857036082985165252462511218699089244945467187571072623490579151099801396985689250933808835629428463352657696286415936289158629585299425218570781030764634615060453765139004555499560932741462331656722996410976468325012470545433354833815198728434027509389192307483003046507871036717299333210733499795748579118306095556829263639
q = 118063811052597078076648057208258290925722946222241910613035885203409338850020920906592375946233241094333077964833028928470213309573854399800445879752747335760515740197890395360028081095608749779850729355973826460560998420377412739941103494034901004876773916365447039754161974845578315476777413979436560683093
e = 65537
ct = 15436002982413531167792439197855366628511742032858020132381247448034583641589139053141197248441750465498638214768245135475992457602527123482967943141258327043927292925664644100351630294623058653796001739830096433745160075993427695839812899234549970407943516319088414287369653326567748224801324184732924590389759109676037235181214031030181051149964539917324617030312886744245699648952675273750261984402368712095508858334065429006024222968042848231102366353871199812809955626892045929470339878801763957059048596399598780789796091488847291534189965452758394821078560041332079868199891048764803339019365629760271406619695

# Calculate the totient of n
phi_n = (q - 1) * ((n // q) - 1)

# Calculate the private exponent d
d = mod_inverse(e, phi_n)

# Decrypt the ciphertext
plaintext = pow(ct, d, n)

# Convert the decrypted plaintext to a string
decrypted_message = bytearray.fromhex(hex(plaintext)[2:]).decode()

print("Decrypted Message:", decrypted_message)

```

## Flag

```text
NC3{s1mp3l_RS4_3r_1ng3n_s4g}
```

## Reflections and Learnings

Working on Rudolf's Simple Algorithm challenge presented an excellent opportunity to dive deep into the fundamentals of RSA encryption, especially the importance of keeping certain elements confidential! The challenge highlighted how revealing even a single prime number used in RSA can completely compromise the security of the encrypted data.

One key takeaway from this challenge was the practical implications of RSA's theoretical security model. RSA's robustness hinges on the difficulty of factoring large numbers into their prime components. However, this challenge served as a reminder that even the most secure systems can be vulnerable if not all components are treated with the same level of confidentiality. In this case, Rudolf's mistake of revealing one of the prime numbers, q, turned the theoretically unbreakable RSA encryption into a simple puzzle.

Solving this challenge reinforced the understanding of the RSA algorithm, particularly how the private key is derived and used for decryption. It was fascinating to see how quickly and efficiently the flag could be decrypted once p was calculated using the given q and n. This process of calculating the private exponent d from e and phi(n) was a practical demonstration of the mathematical principles underlying RSA!