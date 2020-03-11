# Quick-start WordPress with Docker
This self-contained Docker Compose setup will create the following features:
* WordPress installation (localhost, self-signed cert HTTPS available).
* MariaDB database (PMA on :8080).
* Catch-all email service (GUI on :8081).

DB persistence is stored in `db/sys` (created on init). WordPress in `www`.

To start vanilla, run `docker-compose up --build` on the root of this setup (add
`-d` if you wish to close the terminal prompt after).

To start with a pre-existing system, store the WP root directory in `www` and a
database export (.sql) in `db/file`. Start up the system like normal, then exec
into the WordPress container, and run the following command:

`docker-compose exec www wp search-replace domain.com localhost --skip-columns=guid --allow-root`

To run multiple instances, change the left-hand port numbers in
`docker-compose.yml`.

## Troubleshooting and FAQ
### 'Error establishing database connection' on new site
Wait 5-10 minutes after initalising, then run `docker-compose restart www`. It
seems MariaDB (inc MySQL) doesn't finish setup in time for WordPress to begin
initalising. Restarting the WordPress container also restarts the init. This
does not need to be repeated on each restart after initalisation.

Also, if you've started a new project, check to make sure `db/sys` does not
exist before running up, as MariaDB won't install if the directory is not empty.

Further issues can be diagnosed in `logs/debug.log`.

### Starting fresh
Running `docker-compose down` **will not** return the project back to fresh. if
you want to start again, after running said command delete all the `db/sys`
directory (and the contents of `www` if you have a replacement WP package).
