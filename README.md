> Showcase repository: the code and data of this project are private; this README describes it. / Dépôt vitrine : le code et les données de ce projet sont privés ; ce README le décrit.

# Agent fleet management

A coordination post that tracks, across several machines, the coding-agent sessions at work, what they are waiting for and the state of the machines that run them.

## The problem

Anthony runs several agent sessions in parallel on a server, a laptop and shared compute workstations. Without tracking, he had to chase each agent, restart the ones that had stopped and receive results announced too early or wrong. Fleet management centralises this supervision: a dashboard tells him who is working, who is waiting for him, and why.

## How it works

- The dashboard is regenerated every 5 minutes: sessions and their state (working, waiting, paused), status of each project, compute workstations and their reservations, quota, shared decisions, coordination log.
- A summary of each session is written every 30 minutes.
- A request from Anthony goes through a router: it points to the open session concerned, or opens one with a model and an effort level chosen by difficulty (simple, medium, hard).
- A decision that applies to every session is published once, then injected into each session, on each machine.
- Before a result reaches Anthony, a fresh read-only agent checks it; if it disputes the result, the session is restarted for one correction at most.
- Every Monday at 8 am, a weekly report measures session autonomy, restarts, retractions, disputed results and hand-backs to Anthony.

## What is built

In service since 29/10/2026. Functions in place:

- Dashboard, summaries, routing, shared decisions, verification before reporting and weekly report.
- Automatic closing, after one hour, of sessions whose remote-control link is cut (checked every 10 minutes).
- Network watchdog on 3 compute workstations: it checks the link every 5 minutes and repairs it on its own. During test outages, it restored each workstation in 3 min 26 s to 4 min 24 s.

Report of 02/11/2026: overall autonomy of 47 %, 114 restarts, 18 retractions, 64 results disputed by the verifier, 17 hand-backs to Anthony.

Known limit: the evaluation only reads conversations held on the server.

## What is public

This page is public. The dashboard, its code and its data stay private: they describe an institution's machines.

## Tech stack

- Python (dashboard, routing, summaries, report)
- TypeScript (agent tool extensions: shared decisions, verification)
- cron, tmux
- Tailscale
- Windows batch scripts and scheduled tasks (network watchdog)
- HTML and CSS

---

## Version française

# Pilotage de la flotte d'agents

Un poste de coordination qui suit, sur plusieurs machines, les sessions d'agents de programmation au travail, ce qu'elles attendent et l'état des machines qui les portent.

## Le problème

Anthony fait tourner en parallèle plusieurs sessions d'agents sur un serveur, un portable et des postes de calcul partagés. Sans suivi, il devait courir après chaque agent, relancer ceux qui s'étaient arrêtés et recevoir des résultats annoncés trop tôt ou faux. Le pilotage centralise cette supervision : un tableau de bord dit qui travaille, qui l'attend, et pourquoi.

## Comment ça marche

- Le tableau de bord se régénère toutes les 5 minutes : sessions et leur état (travaille, attend, en pause), point de chaque projet, postes de calcul et leurs réservations, quota, décisions communes, journal de la coordination.
- Un résumé de chaque session est écrit toutes les 30 minutes.
- Une demande d'Anthony passe par un aiguillage : il désigne la session ouverte concernée, ou en ouvre une avec un modèle et un niveau d'effort choisis selon la difficulté (simple, moyenne, difficile).
- Une décision qui vaut pour toutes les sessions est diffusée une fois, puis injectée dans chaque session, sur chaque machine.
- Avant qu'un résultat remonte à Anthony, un agent neuf, en lecture seule, le vérifie ; s'il le conteste, la session est relancée pour une correction au plus.
- Chaque lundi à 8 h, un bilan de la semaine mesure l'autonomie des sessions, les relances, les rétractations, les résultats contestés et les renvois vers Anthony.

## Ce qui est construit

En service depuis le 29/10/2026. Fonctions en place :

- Tableau de bord, résumés, aiguillage, décisions communes, vérification avant remontée et bilan hebdomadaire.
- Fermeture automatique, après une heure, des sessions dont le lien de pilotage à distance est coupé (contrôle toutes les 10 minutes).
- Surveillant réseau sur 3 postes de calcul : il vérifie le lien toutes les 5 minutes et le répare seul. Lors des coupures de test, il a rétabli chaque poste en 3 min 26 s à 4 min 24 s.

Bilan du 02/11/2026 : autonomie globale de 47 %, 114 relances, 18 rétractations, 64 résultats contestés par le vérificateur, 17 renvois vers Anthony.

Limite connue : l'évaluation ne lit que les conversations du serveur.

## Ce qui est public

Cette fiche est publique. Le tableau de bord, son code et ses données restent privés : ils décrivent le parc d'un établissement.

## Pile technique

- Python (tableau de bord, aiguillage, résumés, bilan)
- TypeScript (extensions de l'outil d'agents : décisions communes, vérification)
- cron, tmux
- Tailscale
- Scripts batch Windows et tâches planifiées (surveillant réseau)
- HTML et CSS
