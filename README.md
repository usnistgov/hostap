# hostap and wpa\_supplicant with DevID support

DPP hacks for wpa\_supplicant and hostap with device identity.

This fork of hostap adds device identity support during the onboarding process.

The following is a summary of the interactions:

As in standard DPP, the DPP url contains the public key of the device
certificate (not the full certificate). The IOT device (supplicant)
publishes this as a QR code.  This is scanned by Configurator which does
an authentication of the supplicant.  The authentication is based on
the public key of the certificate (not the full certificate).  (The DPP
url itself only contains the raw public key. ) Thus far it is standard
DPP behavior.

After authentication, the supplicant sends a configuration request,
requesting network credentials.

The following enhancement was added to DPP in the hostap submodule.
(NOTE: This is not part of the standard DPP 1.0 protocol.)

The configuration request sent by the supplicant has the IDevID
certificate. The verification step (performed by the Configurator during
processing of the configuration request) checks the certificate to see if
the public key of the DPP url matches the public key of the certificate
and also verifies the certificate chain based on the CA certificate. If
the presented iDevID certificate verifies, the configurator sends network credentials
information to the supplicant, thereby onboarding the supplicant.

The assumptions are :

* The device certificate is assumed be shipped with the device.
* The device has the private key in a tamper proof store and uses this to complete the authentication with the configurator.
  This is the same key that was used to generate the device certificate.
* The configurator has the CA certificate from the manufacturer.


# Building


Copy the .config-hostapd and .config-wpasupplicant into hostapd/ and wpa\_supplicant respectively.

Cd to the respective directories and build as usual.

# Configuring wpa\_supplicant for DevID certifcates

See wpa\_supplicant/README-DPP.md



