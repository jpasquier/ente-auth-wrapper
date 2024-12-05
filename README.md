# Ente Auth Wrapper Script

This script serves as a wrapper for the Ente authentication application,
addressing the Window Height Bug under Linux.

## Purpose

1. **Fix Window Height Bug:**

## Issue

Ente Auth occasionally sets the `flutter.windowHeight` in its configuration
file to an excessively large value (e.g., `67758896.0`), causing a segmentation
fault on startup (https://github.com/ente-io/ente/issues/2326)

## Solution

The script checks the `flutter.windowHeight` value in the configuration file
and resets it to a default value if it exceeds a specified maximum.

## How to Use the Wrapper Script

You can copy the desktop file `/usr/share/applications/enteauth.desktop` into
`$HOME/.local/share/applications/` and replace the `Exec` line with the wrapper
script.

## How It Works


The script reads the Ente Auth configuration file located at:

```
$HOME/.local/share/io.ente.auth/shared_preferences.json
```

It extracts the value of `flutter.windowHeight` and compares it to a maximum
allowed value (`1000.0` by default).

If the value exceeds the maximum, it resets it to a default height (`950.0` by
default).

## Customization

You can change the `MAX_HEIGHT` and `DEFAULT_HEIGHT` variables at the beginning
of the script to suit your preferences.
