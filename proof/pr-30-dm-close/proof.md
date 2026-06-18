# PR #30 DM Close Proof

This proof package shows the browser and API behavior for closing a direct message conversation without deleting it.

## Visual Proof

- `01-dm-visible-before-close.png` - the direct message appears in the sidebar before closing.
- `02-dm-hidden-after-close.png` - closing it removes it from the current user's sidebar.
- `03-hidden-direct-route-opens.png` - the hidden DM can still be opened directly by route/link.
- `04-reopen-reuses-and-unhides.png` - starting the same one-to-one DM again reuses the same conversation and shows it again.
- `05-new-message-resurfaces-dm.png` - a new root DM message from the other user resurfaces the hidden DM with one unread message.
- `06-resurfaced-message-opened.png` - selecting the resurfaced DM opens the new message.

## Current Head Menu + Undo Proof

Additional proof was captured for PR head `406c37cdc7cc7729274077165ae6472d82683dde`, after the final actions-menu and Undo follow-up:

- `07-actions-menu-open.png` - the direct-message row exposes the `...` actions menu and shows the `Close direct message` menu item.
- `08-undo-visible-after-close.png` - closing the DM hides it from the direct-message list and shows the inline `Undo` affordance.
- `09-undo-restores-same-dm.png` - pressing `Undo` restores the same DM to the direct-message list.

Backing current-head proof is in `proof-current-head-menu-undo.json`:

- Conversation: `dm_01kvdqdq3wptn9j4c0vbkm5dqb`
- Route id: `DZYM1106MB26XCME9`
- Hidden direct `GET /api/dms/{id}` status: `200`
- Undo `POST /api/dms/{id}/open` status: `200`
- Undo restored same conversation: `true`

`server-current-head.log` includes the matching local proof request sequence:

- `DELETE /api/dms/dm_01kvdqdq3wptn9j4c0vbkm5dqb -> 200`
- `GET /api/dms/dm_01kvdqdq3wptn9j4c0vbkm5dqb -> 200`
- `POST /api/dms/dm_01kvdqdq3wptn9j4c0vbkm5dqb/open -> 200`

## Backing Runtime Data

- Workspace: `wsp_01kvazkkrzn50axyxgn7kvgybx`
- Other user: `usr_01kvazkm0bqybjzvz1qam802fm`
- Conversation: `dm_01kvazkm960f33k8z52b90m9rn`
- Route id: `DVCQXX6JHJJFBZ6X4`
- Reopened same conversation: `true`
- Resurface message: `msg_01kvazkmmfb6vnnfw7nj4yw741`
- Resurface event: `evt_01kvazkmmfb6vnnfw7nmktx6hk`
- Event type: `message.created`
- Event sequence: `1`
- Message body: `resurface this dm`
- Event timestamp: `2026-06-17T14:26:19.919332Z`

## Server Log Evidence

The captured `server.log` includes the matching request sequence:

- `POST /api/dms -> 201`
- `DELETE /api/dms/dm_01kvazkm960f33k8z52b90m9rn -> 200`
- `GET /api/dms/dm_01kvazkm960f33k8z52b90m9rn -> 200`
- hidden list state: `GET /api/dms -> 200 21B`
- reopen state: `POST /api/dms -> 201`
- resurface message: `POST /api/dms/dm_01kvazkm960f33k8z52b90m9rn/messages -> 201`
- resurfaced list state: `GET /api/dms -> 200 558B`
- opened message list: `GET /api/dms/dm_01kvazkm960f33k8z52b90m9rn/messages -> 200 601B`

Full captured API responses are in `proof-log.json`.
