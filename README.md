# Ente Auth Wrapper Script

This script serves as a wrapper for the Ente authentication application,
addressing two issues on Linux systems.

## Purpose

1. **Fix Window Height Bug:**

   - **Issue:** Ente Auth occasionally sets the `flutter.windowHeight` in its
     configuration file to an excessively large value (e.g., `67758896.0`),
     causing a segmentation fault on startup
     (https://github.com/ente-io/ente/issues/2326)
   - **Solution:** The script checks the `flutter.windowHeight` value in the
     configuration file and resets it to a default value if it exceeds a
     specified maximum.

2. **Relocate Database File:**

   - **Issue:** Ente Auth stores its database in a non-standard location
     (`$HOME/Documents/ente/.ente.authenticator.db`)
   - **Solution:** The script creates a symbolic link to the database file
     stored in a more conventional directory
     (`$HOME/.local/share/io.ente.auth/.ente.authenticator.db`). The link is
     created and removed at each call of the wrapper script.

## How to Use the Wrapper Script

You can copy the desktop file `/usr/share/applications/ente_auth.desktop` into
`$HOME/.local/share/applications/` and replace the `Exec` line with the wrapper
script.

## How It Works

1. **Window Height Adjustment:**

   - The script reads the Ente Auth configuration file located at:

     ```
     $HOME/.local/share/io.ente.auth/shared_preferences.json
     ```

   - It extracts the value of `flutter.windowHeight` and compares it to a
     maximum allowed value (`1000.0` by default).
   - If the value exceeds the maximum, it resets it to a default height
     (`900.0` by default).

2. **Database File Symlink:**

   - Creates a directory at `$HOME/Documents/ente/` if it doesn't exist (this
     is where Ente Auth stores its database file by default).
   - Checks that the directory is empty to avoid overwriting existing files.
   - Creates a symbolic link to the Ente Auth database file:

     ```
     $HOME/Documents/ente/.ente.authenticator.db -> $HOME/.local/share/io.ente.auth/.ente.authenticator.db
     ```

   - After `ente_auth` exits, the script removes the symbolic link and, if the
     directory is empty, removes it.

## Customization

- **Adjust Maximum and Default Window Heights:**

  You can change the `MAX_HEIGHT` and `DEFAULT_HEIGHT` variables at the
  beginning of the script to suit your preferences.

- **Change Database Location:**

  Modify the `DATABASE_FILE` variable to change where the database is stored.
