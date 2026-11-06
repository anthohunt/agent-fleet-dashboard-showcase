[English](README.md) · **Français**

**Anthony Hunt** · [Fiche du projet sur ingevision.cloud](https://ingevision.cloud/?p=pilotage-flotte) · [LinkedIn](https://www.linkedin.com/in/anthony-hunt-3948a9196) · [Profil GitHub](https://github.com/anthohunt)

> Dépôt vitrine : le code et les données de ce projet sont privés ; ce README le décrit.

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
