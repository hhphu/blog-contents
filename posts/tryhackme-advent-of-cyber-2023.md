---
title: "TryHackMe — Advent of Cyber 2023"
description: "A 24-day series of beginner-friendly security challenges hosted by TryHackMe in December 2023."
date: "2024-07-26"
headerImage: "https://miro.medium.com/v2/resize:fit:720/format:webp/0*MtYnbQiBxBCkGwty.png"
tags: ["TryHackMe"]
---

# INTRODUCTION
Advent of Cyber 2023 was a free, 24-day series of beginner-friendly security challenges hosted by TryHackMe that took place in December 2023 leading up to Christmas. Each day, participants completed a new challenge that covered topics such as: Penetration testing, Security operations, Security engineering, Digital forensics, Incident response, Machine learning, and Malware analysis.

This blog post serves as an index that makes it easier for me to navigate amongst the rooms

# Table of Contents
- Day 1: Chatbot, tell me if you’re really safe? - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-1-chatbot-tell-me-if-youre-really-safe-a17a1b08ebaf)
- Day 2: O Data, All Ye Faithful - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-2-log-analysis-o-data-all-ye-faithful-2c014fec871b)
- Day 3: Hydra is Coming to Town - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-3-brute-forcing-hydra-is-coming-to-town-b7069e1c6dbb)
- Day 4: Baby, It’s CeWLd outside - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-4-brute-forcing-baby-its-cewld-outside-5d9873cbfd6f)
- Day 5: A Christmas DOScovery: Tapes of Yule-tide-Past - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-5-doscovery-tapes-of-yule-tide-past-90eb327a1727)
- Day 6: Memories of Christmas Past - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-6-memory-corruption-memories-of-christmas-past-138d10a6ea32)
- Day 7: ’Tis the season for log chopping! - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-7-log-analysis-tis-the-season-for-log-chopping-63a4bd5dd106)
- Day 8: Have a Holly, Jolly Byte! - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-8-disk-forensic-have-a-holly-jolly-byte-fe87922649b2)
- Day 9: She sells C# shells by the C2shore - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-9-malware-analysisp-she-sells-c-shells-by-the-c2shore-c2b9020a5485)
- Day 10: Inject the Halls with EXEC Queries - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-10-sql-injection-injets-the-halls-with-exec-queries-1f9aa9cd4ead)
- Day 11: Jingle Bells, Shadow Spells - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-11-active-directory-jingle-bells-shadow-spells-c463c660d462)
- Day 12: Sleighing Threats, One Layer at a Time - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-11-defense-in-depth-sleighing-threats-one-layer-at-a-time-849cf4e2a00c)
- Day 13: To the Pots, Through the Walls - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-13-intrusion-detection-to-the-pots-through-the-walls-1d7338de213a)
- Day 14: The Little Machine That Wanted to Learn - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-14-machine-learning-the-little-machine-that-wanted-to-515df850bdad)
- Day 15: Jingle Bell SPAM: Machine Learning Saves the Day! - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-16-machine-learning-jingle-bells-spam-machine-learning-0a0f6b820cd8)
- Day 16: Can’t CAPTCHA this Machine! - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-16-machine-learning-cant-captcha-this-machine-55e139ee0825)
- Day 17: I Tawt I Taw A C2 Tat! - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-17-traffic-analysis-tawt-i-taw-a-c2-tat-f006144123b4)
- Day 18: A Gift That Keeps on Giving - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-18-eradication-the-gift-that-keeps-on-giving-610ef7122b90)
- Day 19: CrypTOYminers Sing Volala-lala-latility - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-19-memory-forensics-cryptoyminers-sing-dd9e9535d3c3)
- Day 20: Advent of Frostlings - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-20-devsecops-advent-of-frostlings-151309650a27)
- Day 21: Yule be Poisoined: A Pipeline of Insecure Code! - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-21-devsecops-yule-be-poisoned-a-pipeline-of-insecure-code-0433ca0768dd)
- Day 22: Jingle Your SSRF Bells: a Merry Command & Control Hackventure - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-21-ssrf-jingle-your-ssrf-bells-a-merry-command-control-0dacd45e4edb)
- Day 23: Relay All the Way - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-23-coerced-authentication-relay-all-the-way-5c165fad4bdf)
- Day 24: You Are on the Naughty List, McGreedy - [Link](https://medium.com/@hhphu/tryhackme-advent-of-cyber-2023-day-24-mobile-analysis-you-are-on-a-naughty-list-mcgreedy-1b61ac0b4b4e)

# CONCLUSION
Hope that after going through these rooms, you’ll learn more about Cybersecurity.
Please clap if you like this post. And don’t forget to follow me for more Cybersecurity.
