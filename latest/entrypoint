#!/bin/sh
set -eo pipefail

supervisord -c /etc/supervisord.conf

crond

# first arg is `-f` or `--some-option`s
if [ "${1#-}" != "$1" ]; then
	set -- php-fpm "$@"
fi

exec "$@"