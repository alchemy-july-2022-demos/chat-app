# Realtime

Realtime is now enabled by default for new projects!

## Turn on for Database

But here are instructions Just in Case:

[Instructions](https://supabase.com/docs/guides/api#realtime-api-1)

## RLS

There is [currently a bug in supabase](https://github.com/supabase/realtime/issues/265) for using RLS with realtime.

So turn of RLS for tables that you want to listen to, or follow the suggested work around in that issue!