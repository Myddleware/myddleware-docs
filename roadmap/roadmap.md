# Project Roadmap

```mermaid
gantt
    title Myddleware Enhancement Project Timeline
    dateFormat YYYY-MM-DD
    axisFormat %B %Y

    section Sprint 1
    Connecteur Act-On       :2025-03-12, 2025-04-25
    Connecteur SuiteCRM    :2025-03-12, 2025-04-25
    Upgrade Symphony       :2025-03-12, 2025-04-25
    Upgrade librairies    :2025-03-12, 2025-04-25
    Double auth           :2025-03-12, 2025-04-25
    Connecteur MintHCM    :2025-03-12, 2025-04-25

    section Sprint 2 (April-June 2025)
    Refonte profil        :2025-04-25, 2025-06-04
    Vue documents         :2025-04-25, 2025-06-04
    Gestion règles       :2025-04-25, 2025-06-04
    Transport envir      :2025-04-25, 2025-06-04

    section Sprint 3 (June-July 2025)
    Terminal             :2025-06-04, 2025-07-09
    Gestion users        :2025-06-04, 2025-07-09
    Class. règles        :2025-06-04, 2025-07-09
    End date jobs        :2025-06-04, 2025-07-09

    section Sprint 4 (July-August 2025)
    Refonte création règles   :2025-07-09, 2025-08-27
    Refonte modif règles     :2025-07-09, 2025-08-27
    Refonte vue règles       :2025-07-09, 2025-08-27
    Document cleaning        :2025-07-09, 2025-08-27

    section Sprint 5 (August-October 2025)
    Refonte workflows        :2025-08-27, 2025-10-15
    Duplication workflows    :2025-08-27, 2025-10-15
    Helper conditions        :2025-08-27, 2025-10-15
    Rerun workflow actions   :2025-08-27, 2025-10-15
    Error transform fields   :2025-08-27, 2025-10-15

    section Sprint 6 (October-November 2025)
    Création listes        :2025-10-15, 2025-11-19
    Change value          :2025-10-15, 2025-11-19
    Duplic. tables        :2025-10-15, 2025-11-19
    Rerun mass           :2025-10-15, 2025-11-19
    Favoris docs         :2025-10-15, 2025-11-19
    Evolution search     :2025-10-15, 2025-11-19

    section Sprint 7 (July 2025-January 2026)
    Monitoring/Reports       :2025-07-07, 2026-01-21
```

This Gantt chart shows:
1. All tasks organized by sprints
2. Actual start and end dates from the CSV
3. Clear section headers with date ranges
4. Tasks grouped logically by sprint
5. Monitoring/Reports shown separately as it spans multiple sprints

The chart uses:
- YYYY-MM-DD date format for precision
- Month Year axis format for readability
- Sprint-based sections for organization
- Task durations based on the CSV dates

This creates a basic Gantt chart with:
1. A title
2. Date formatting set to YYYY-MM (year-month)
3. Axis labels showing month names
4. Tasks grouped into sections by month
5. Each task showing its duration (1M means one month)

The syntax breakdown:
- `gantt`: Declares this is a Gantt chart
- `dateFormat`: Specifies how dates should be interpreted
- `axisFormat`: Controls how dates are displayed on the axis
- `section`: Groups related tasks
- Task syntax: `Task name : start_date, duration`

You can preview this in any markdown editor that supports Mermaid diagrams. Would you like to:
1. Add more tasks to this basic structure?
2. Adjust the styling?
3. Add more months to the timeline?

Let me know how you'd like to proceed and we can build upon this foundation.