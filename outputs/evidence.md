# Recorded experiment evidence

Generated from actual notebook outputs. This is not the student report.

## q28: Can users be asked for their first pet's name when selecting a login password?

Category: keyword_trap; critical PDF page: 25.
Critical-evidence rank: 5 → 1.

Source checklist: No prompting for specific personal information such as first-pet names.

### Answer 1 — baseline

No, users cannot be asked for their first pet's name when selecting a login password according to the passage. The text states that "users' password choices are very predictable" and mentions examples like "Password1!" and "Password1!", indicating that common patterns exist and suggest avoiding certain inputs. Therefore, asking for personal details like a pet's name could lead to predictable passwords being chosen.

### Answer 2 — re-ranked

No, according to the passage, users shall not be prompted to use specific types of information when choosing memorized secrets, including asking about their first pet's name. The passage states "Verifiers SHALL NOT prompt subscribers to use specific types of information" when choosing memorized secrets.

## q02: Do users need to change their password every month even when it has not leaked?

Category: exceptions; critical PDF page: 25.
Critical-evidence rank: 7 → 3.

Source checklist: No arbitrary periodic changes; force a change when compromise is evidenced.

### Answer 1 — baseline

No, according to the passage, users typically do not need to change their password every month even if it has been compromised. The text states "The ability of the attacker to determine one or more users' passwords depends on the way in which the password is stored," implying that once a password is known, changing it immediately after a leak does not significantly enhance security. Therefore, while it may be prudent to change passwords regularly, especially due to other factors like expiration dates, immediate changes following a compromise are not mandated.

### Answer 2 — re-ranked

No, according to the passage, users should not be required to change their memorized secrets arbitrarily unless there is a user request or evidence of authenticator compromise. Therefore, changing passwords monthly without such justification would violate this principle.

## q11: What is the deadline for finishing an out-of-band authentication transaction?

Category: document_specific; critical PDF page: 29.
Critical-evidence rank: 6 → 1.

Source checklist: The authentication is invalid if not completed within 10 minutes.

### Answer 1 — baseline

The deadline for finishing an out-of-band authentication transaction depends on the specific conditions outlined in the passages. According to [p029_c02], if using a secure application like a smart phone, the verifier may send a push notification to the device, which the verifier then waits for the establishment of an authenticated protected channel and verifies the authenticator's identifying key. After successful authentication, the verifier transmits the authentication secret to the authenticator. However, the passage does not specify a strict deadline beyond this process; it emphasizes periodic reauthentication as described in [p021_c01]. Therefore, while there isn't a single, universal deadline mentioned, the process involves multiple steps including initial authentication and periodic reauthentication checks.

### Answer 2 — re-ranked

The deadline for completing an out-of-band authentication transaction is within 10 minutes according to the specified rules [p029_c03].

## q06: Can several blank spaces in a passphrase be collapsed, and what length must remain?

Category: exceptions; critical PDF page: 24.
Critical-evidence rank: 6 → 1.

Source checklist: Consecutive spaces may become one only if the result is at least 8 characters; no truncation.

### Answer 1 — baseline

Several blank spaces in a passphrase can be collapsed, but the remaining length must be sufficient to maintain the intended phrase's integrity. According to the passage, "Users' password choices are very predictable," which means that collapsing multiple spaces could potentially lead to confusion or misuse of the passphrase. Therefore, the remaining length after collapsing spaces should ensure that the original intent of the passphrase remains clear and usable.

### Answer 2 — re-ranked

According to the given passages, verifiers MAY replace multiple consecutive space characters with a single space character prior to verification, but truncation of the secret SHALL NOT be performed. The minimum required length for a memorized secret remains at least 64 characters, which supports the use of passphrases.

## q13: Does the general limit of 100 wrong guesses also allow 100 failed fingerprint scans?

Category: exceptions; critical PDF page: 38.
Critical-evidence rank: 5 → 2.

Source checklist: No; biometric-specific limits are 5, or 10 with qualifying PAD, followed by specified mitigation.

### Answer 1 — baseline

No, the general limit of 100 wrong guesses does not apply to fingerprint scans. According to [p036_c04], the verifier shall limit consecutive failed authentication attempts on a single account to no more than 100, but this is specifically for other types of authenticators like passwords or tokens, not for fingerprint scans.

### Answer 2 — re-ranked

No, the general limit of 100 wrong guesses does not apply to fingerprint scans specifically. According to [p036_c04], unless otherwise specified in the description of a given authenticator, the verifier SHALL limit consecutive failed authentication attempts on a single account to no more than 100. This applies to all types of authentication methods, including fingerprints. Therefore, while there might be additional restrictions based on the specific implementation details, the general limit of 100 wrong guesses does not directly translate to a separate limit for fingerprint scans.

## q29: Can a passphrase with repeated spaces be normalized even though truncation is prohibited?

Category: exceptions; critical PDF page: 24.
Critical-evidence rank: 4 → 1.

Source checklist: Space collapsing is permitted provided the result is at least 8 characters; truncation remains prohibited.

### Answer 1 — baseline

Yes, a passphrase with repeated spaces can be normalized because truncation is prohibited according to the passage [p079_c02], which states "Users should also be able to include space characters to allow the use of phrases." Therefore, repeating spaces does not violate this restriction.

### Answer 2 — re-ranked

Yes, according to the passage, if Unicode characters are accepted in memorized secrets, the verifier SHOULD apply the Normalization Process for Stabilized Strings using either the NFKC or NFKD normalization defined in Section 12.1 of Unicode Standard Annex 15 [UAX 15], which applies before hashing the byte string representing the memorized secret. This allows for normalizing spaces without violating the prohibition against truncation.

## q25: Are a user's face and fingerprints secret in the same way as a password?

Category: keyword_trap; critical PDF page: 37.
Critical-evidence rank: 1 → 5.

Source checklist: No; biometric characteristics can be captured or obtained without the user's knowledge.

### Answer 1 — baseline

No, a user's face and fingerprints are not considered secrets in the same way as a password. According to the passage [p037_c03], "Biometric characteristics do not constitute secrets." This means that unlike passwords, which are typically kept secret and protected, biometric data like faces and fingerprints can be easily obtained through various means such as photographs or scans.

### Answer 2 — re-ranked

No, a user's face and fingerprints are not considered secrets in the same way as a password according to the given passages. Passwords are typically memorized and subject to change, while faces and fingerprints are biometric identifiers that are unique to individuals and cannot be forgotten or changed. The passages emphasize the limitations of human memory in remembering complex secrets and suggest alternative methods like password managers to enhance security without requiring constant changes.

## q09: Does a locked phone need to reveal the one-time login code on its screen?

Category: negation; critical PDF page: 28.
Critical-evidence rank: 3 → 1.

Source checklist: It SHOULD NOT display the secret while locked, but SHOULD indicate receipt.

### Answer 1 — baseline

No, according to the passage [p020_c05], when a device like a smartphone is used in the authentication process, the unlocking of that device shall not be considered one of the authentication factors. Therefore, a locked phone does not need to reveal the one-time login code on its screen.

### Answer 2 — re-ranked

A locked phone does not need to reveal the one-time login code on its screen according to the first passage [p028_c04]. The passage states that if a secret is sent by the verifier to the out-of-band device, the device SHOULD NOT display the authentication secret while it is locked by the owner. Therefore, even though the phone might be locked, the secret itself would not appear on the screen.
