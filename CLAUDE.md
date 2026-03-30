# CLAUDE.md — revisionable

## Overview

Ubiquilife fork of VentureCraft's Revisionable. Automatically tracks field-level change history on Eloquent models. Every QoModel in Appbase uses this to provide an audit trail — who changed what, when, and the old/new values.

## Namespace

`Ubiquilife\Revisionable` (PSR-0 autoloading: `src/Ubiquilife/Revisionable/`)

## Key Classes

- **`RevisionableTrait`** — The main trait. Add to any model to enable revision tracking. Hooks into Eloquent `creating`, `updating`, `deleting` events. Stores diffs in the `revisions` table.
- **`Revision`** — Eloquent model for the `revisions` table. Fields: `revisionable_type`, `revisionable_id`, `user_id`, `key`, `old_value`, `new_value`.
- **`Revisionable`** — Abstract base model (alternative to using the trait directly).
- **`FieldFormatter`** — Formats revision values for display (dates, booleans, relationships).
- **`RevisionableServiceProvider`** — Auto-discovered. Publishes migration.

## Model Configuration

```php
protected $revisionEnabled = true;
protected $dontKeepRevisionOf = ['updated_at'];  // Fields to ignore
protected $revisionFormattedFieldNames = ['title' => 'Title'];  // Display names
protected $historyLimit = 500;  // Max revisions per record
```

## Testing

```bash
cd revisionable && vendor/bin/phpunit
```

## Mandatory Rules

- This is a PRIVATE Ubiquilife package. Changes affect ALL apps.
- NEVER change the `revisions` table schema without a migration — every app has this table.
- NEVER change `RevisionableTrait` method signatures without checking Appbase's QoModel.
- The migration lives in this package — do NOT create duplicate migrations in consuming apps.
- Use British spelling in all text and comments.
- Test before committing.
- One logical change per commit.
