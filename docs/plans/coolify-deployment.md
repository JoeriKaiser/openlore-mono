## Deployment Steps

1. Push code to Git repository
2. In Coolify, create new "Docker Compose" resource
3. Connect to Git repository
4. Set compose file path to `docker-compose.coolify.yml`
5. Add all environment variables in Coolify UI
6. Configure domains in Coolify (or rely on Traefik labels)
7. Deploy

## Backup Considerations

Since PostgreSQL is in the compose stack:
- Set up periodic volume backups
- Consider Coolify's backup features for volumes
- Or use `pg_dump` via scheduled task
