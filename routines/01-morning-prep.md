ROUTINE 1 of 3: MORNING PREP (runs 5:00 AM)

CONTEXT: I am a Euna Solutions AE. My time zone is Eastern
(America/New_York).
Use the Salesforce connector. Get my user Id with get_user_info.
This run is READ-ONLY: never create, update, or delete Salesforce,
Salesloft, or Veles records, and never send an email. Flag anything
unverified.

HOW TO FIND MY MEETINGS (Salesforce Event):
- Events I own (OwnerId = me) plus Events where I am an invitee
  (EventRelation.RelationId = me). StartDateTime is stored in UTC;
  convert to my time zone.
- Merge duplicates: '[Gong] ...' and 'Meeting Booked: ...' events for
  the same account/opportunity are ONE meeting. If their times differ,
  trust the '[Gong]' event (it mirrors my live calendar); the other
  is usually a meeting that was rescheduled.
- Cross-check my Outlook calendar (read-only) for external attendees
  missing from Salesforce. List any gaps; do not log them.
- Skip internal-only meetings (no Contact, Account, Opportunity, or
  non-Euna attendee).

MEETING TYPES (classify every external meeting into ONE):
| Type         | Signals (subject, stage, history)                       |
|--------------|---------------------------------------------------------|
| Discovery    | intro, discovery, first meeting; no prior Gong call     |
| Demo         | demo, walkthrough, product overview                     |
| Demo Debrief | debrief, recap, feedback; within 14 days after a demo   |
| Pricing      | pricing, quote, proposal, budget review                 |
| Follow-up    | follow up, reconnect, check-in, next steps              |
| Contracting  | contract, order form, legal, redlines, signature, SOW   |
| Other        | external but none of the above (kickoff, renewal, etc.) |
If signals conflict, use the opportunity StageName and the last Gong
call to decide, and state why. If still unclear, say so.

SHARED PREP (every external meeting):
- Account, opportunity (stage, amount, close date, forecast category),
  Next_Step_bf__c and Next_Step_Date_bf__c, MEDDPICC fields.
- Last 90 days of Gong__Gong_Call__c briefs on the account and the
  most recent Salesloft calls/emails with each attendee.
- Attendees: name, title, role in the deal (champion, economic buyer,
  evaluator, blocker, unknown), first time in a meeting or not.
- What I committed to on the last call and whether it is done.

TYPE-SPECIFIC PREP:
1. Discovery: run /pre-discovery-research for the account and every
   attendee (role, background, recent news, Salesforce history).
2. Demo: SE brief: their pain in their words, the people, what to
   show, what to avoid, open questions. Source: Gong briefs (90 days),
   MEDDPICC, Next Step fields.
3. Demo Debrief: recap of the demo call (what landed, what fell flat,
   objections raised, who reacted how), MEDDPICC gaps the demo did not
   close, and the path from here to pricing.
4. Pricing: current Veles proposal and quote (products, quantities,
   discounts, total contract value, first-year value, approval status;
   run validate_quote read-only). Compare to stated budget and
   Decision Criteria. List expected pushback and my walk-away guardrails
   (list_pricing_rules). Flag if no quote exists.
5. Follow-up: what changed since the last meeting, open action items
   on both sides, and the single decision this meeting must produce.
6. Contracting: Veles order/contract status and next_step, paper
   process (MEDDPICC), procurement vehicle (co-op, RFP, sole source),
   signatory vs. Economic Buyer, legal or redline issues from Gong or
   email, and close-date risk. Name the deal-support contact for any
   open legal, PS, or finance item.
7. Other: shared prep only, plus the stated purpose of the meeting.

BOOKING TOUCH (Discovery and Demo only):
Find who booked the meeting (CreatedBy on the 'Meeting Booked' Event;
the account's BDR_Territory_Owner__c). Find the call or email that
booked it in the 30 days before the Event was created:
Gong__Gong_Call__c on the account, Salesloft calls and emails to the
attendee. Run /call-scorecard on it. If none is found, write 'No
booking touch found' and list where you looked. Do not guess.

LAST-CALL CHECK (Demo Debrief, Pricing, Follow-up, Contracting):
Run /call-scorecard on the most recent external call on this
opportunity. Report ANT and the top coaching gap only. If none is
found, write 'No prior call found' and list where you looked.

LEFTOVERS: for any external meeting YESTERDAY that started after
4:00 PM, run the follow-up steps from Routine 2.

OUTPUT (one brief per meeting, earliest first):
- Time, type, account, attendees, Salesforce Opportunity link
- Meeting goal: the one outcome that makes this call a win
- Scorecard: booking touch (Discovery/Demo) or last call (others):
  ANT scores (Authority, Need, Timeline), what the prospect said, gaps
- 3-5 open-ended clarification questions to open the call, tuned to
  the meeting type
- Type-specific prep highlights (research, SE brief, debrief, pricing,
  follow-up, or contracting)
- Risks and anything unverified
Start with a 5-line summary of the whole day, including a count of
meetings by type.
