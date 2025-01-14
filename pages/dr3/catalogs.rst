.. title: Tractor Catalog Format
.. slug: catalogs
.. tags: 
.. has_math: yes

.. |chi|      unicode:: U+003C7 .. GREEK SMALL LETTER CHI
.. |sup2|   unicode:: U+000B2 .. SUPERSCRIPT TWO
.. |epsilon|  unicode:: U+003B5 .. GREEK SMALL LETTER EPSILON
.. |phi|      unicode:: U+003D5 .. GREEK PHI SYMBOL
.. |deg|    unicode:: U+000B0 .. DEGREE SIGN
.. |Prime|    unicode:: U+02033 .. DOUBLE PRIME

tractor/<AAA>/tractor-<brick>.fits
----------------------------------

FITS binary table containing Tractor photometry. Before using these catalogs, note that there are
`known issues`_ regarding their content and derivation.

.. _`known issues`: ../issues

=========================== ============ ===================== ===============================================
Name                        Type         Units                 Description
=========================== ============ ===================== ===============================================
BRICKID                     int32                              Brick ID [1,662174]
BRICKNAME                   char                               Name of brick, encoding the brick sky position, eg "1126p222" near RA=112.6, Dec=+22.2
OBJID                       int32                              Catalog object number within this brick; a unique identifier hash is BRICKID,OBJID;  OBJID spans [0,N-1] and is contiguously enumerated within each brick
BRICK_PRIMARY               boolean                            True if the object is within the brick boundary
BLOB                        int32                              Blend family; objects with the same [BRICKID,BLOB] identifier were modeled (deblended) together; contiguously numbered from 0
NINBLOB                     int16                              Number of sources in this BLOB (blend family); isolated objects have value 1.
TYCHO2INBLOB                boolean                            Is there a Tycho-2 (very bright) star in this blob?
TYPE                        char[4]                            Morphological model: "PSF"=stellar, "SIMP"="simple galaxy" = 0.45" round EXP galaxy, "DEV"=deVauc, "EXP"=exponential, "COMP"=composite.  Note that in some FITS readers, a trailing space may be appended for "PSF ", "DEV " and "EXP " since the column data type is a 4-character string
RA                          float64      deg                   Right ascension at equinox J2000
RA_IVAR                     float32      1/deg\ |sup2|         Inverse variance of RA (no cosine term!), excluding astrometric calibration errors
DEC                         float64      deg                   Declination at equinox J2000
DEC_IVAR                    float32      1/deg\ |sup2|         Inverse variance of DEC, excluding astrometric calibration errors
BX                          float32      pix                   X position (0-indexed) of coordinates in brick image stack
BY                          float32      pix                   Y position (0-indexed) of coordinates in brick image stack
BX0                         float32      pix                   Initialized X position (0-indexed) of coordinates in brick image stack
BY0                         float32      pix                   Initialized Y position (0-indexed) of coordinates in brick image stack
LEFT_BLOB                   boolean                            True if an object center has been optimized to be outside the fitting pixel area
OUT_OF_BOUNDS               boolean                            True for objects whose center is on the brick; less strong of a cut than BRICK_PRIMARY
DCHISQ                      float32[5]                         Difference in |chi|\ |sup2| between successively more-complex model fits: PSF, SIMPle, DEV, EXP, COMP.  The difference is versus no source.
EBV                         float32      mag                   Galactic extinction E(B-V) reddening from SFD98, used to compute DECAM_MW_TRANSMISSION and WISE_MW_TRANSMISSION
CPU_SOURCE                  float32      seconds               CPU time used for fitting this source
CPU_BLOB                    float32      seconds               CPU time used for fitting this blob of sources (all sources in this brick with the same blob number)
BLOB_WIDTH                  int16                              Size of this blob of pixels in brick coordinates, bounding box width
BLOB_HEIGHT                 int16                              Size of this blob of pixels in brick coordinates, bounding box height
BLOB_NPIX                   int32                              Size of this blob of pixels in brick coordinates, number of brick pixels
BLOB_NIMAGES                int16                              Number of images overlapping this blob
BLOB_TOTALPIX               int32                              Total number of pixels from all the images overlapping this blob
DECAM_FLUX                  float32[6]   nanomaggy             DECam model flux in ugrizY
DECAM_FLUX_IVAR             float32[6]   1/nanomaggy\ |sup2|   Inverse variance oF DECAM_FLUX
DECAM_APFLUX                float32[8,6] nanomaggy             DECam aperture fluxes on the co-added images in apertures of radius  [0.5,0.75,1.0,1.5,2.0,3.5,5.0,7.0] arcsec in ugrizY
DECAM_APFLUX_RESID          float32[8,6] nanomaggy             DECam aperture fluxes on the co-added residual images
DECAM_APFLUX_IVAR           float32[8,6] 1/nanomaggy\ |sup2|   Inverse variance oF DECAM_APFLUX
DECAM_MW_TRANSMISSION       float32[6]                         Galactic transmission in ugrizY filters in linear units [0,1]
DECAM_NOBS                  uint8[6]                           Number of images that contribute to the central pixel in each filter for this object (not profile-weighted)
DECAM_RCHI2                 float32[6]                         Profile-weighted |chi|\ |sup2| of model fit normalized by the number of pixels
DECAM_FRACFLUX              float32[6]                         Profile-weight fraction of the flux from other sources divided by the total flux (typically [0,1])
DECAM_FRACMASKED            float32[6]                         Profile-weighted fraction of pixels masked from all observations of this object, strictly between [0,1]
DECAM_FRACIN                float32[6]                         Fraction of a source's flux within the blob, near unity for real sources
DECAM_ANYMASK               int16[6]                           Bitwise mask set if the central pixel from any image satisfy each condition
DECAM_ALLMASK               int16[6]                           Bitwise mask set if the central pixel from all images satisfy each condition
DECAM_PSFSIZE               float32[6]   arcsec                Weighted average PSF FWHM per band
WISE_FLUX                   float32[4]   nanomaggy             WISE model flux in W1,W2,W3,W4
WISE_FLUX_IVAR              float32[4]   1/nanomaggy\ |sup2|   Inverse variance of WISE_FLUX
WISE_MW_TRANSMISSION        float32[4]                         Galactic transmission in W1,W2,W3,W4 filters in linear units [0,1]
WISE_NOBS                   int16[4]                           Number of images that contribute to the central pixel in each filter for this object (not profile-weighted)
WISE_FRACFLUX               float32[4]                         Profile-weight fraction of the flux from other sources divided by the total flux (typically [0,1])
WISE_RCHI2                  float32[4]                         Profile-weighted |chi|\ |sup2| of model fit normalized by the number of pixels
WISE_LC_FLUX                float32[5,2] nanomaggy             Analog of WISE_FLUX, for each of up to five unWISE coadd epochs; W1 and W2 only
WISE_LC_FLUX_IVAR           float32[5,2] 1/nanomaggy\ |sup2|   Analog of WISE_FLUX_IVAR, for each of up to five unWISE coadd epochs; W1 and W2 only
WISE_LC_NOBS                int16[5,2]                         Analog of WISE_NOBS, for each of up to five unWISE coadd epochs; W1 and W2 only
WISE_LC_FRACFLUX            float32[5,2]                       Analog of WISE_FRACFLUX, for each of up to five unWISE coadd epochs; W1 and W2 only
WISE_LC_RCHI2               float32[5,2]                       Analog of WISE_RCHI2, for each of up to five unWISE coadd epochs; W1 and W2 only
WISE_LC_MJD                 float32[5,2]                       Mean MJD in W1 and W2, for up to five unWISE coadd epochs; 0 means epoch unavailable
FRACDEV                     float32                            Fraction of model in deVauc [0,1]
FRACDEV_IVAR                float32                            Inverse variance of FRACDEV
SHAPEEXP_R                  float32      arcsec                Half-light radius of exponential model (>0)
SHAPEEXP_R_IVAR             float32      1/arcsec\ |sup2|      Inverse variance of R_EXP
SHAPEEXP_E1                 float32                            Ellipticity component 1
SHAPEEXP_E1_IVAR            float32                            Inverse variance of SHAPEEXP_E1
SHAPEEXP_E2                 float32                            Ellipticity component 2
SHAPEEXP_E2_IVAR            float32                            Inverse variance of SHAPEEXP_E2
SHAPEDEV_R                  float32      arcsec                Half-light radius of deVaucouleurs model (>0)
SHAPEDEV_R_IVAR             float32      1/arcsec\ |sup2|      Inverse variance of R_DEV
SHAPEDEV_E1                 float32                            Ellipticity component 1
SHAPEDEV_E1_IVAR            float32                            Inverse variance of SHAPEDEV_E1
SHAPEDEV_E2                 float32                            Ellipticity component 2
SHAPEDEV_E2_IVAR            float32                            Inverse variance of SHAPEDEV_E2
DECAM_DEPTH                 float32[6]   1/nanomaggy\ |sup2|   For a :math:`5\sigma` point source detection limit, :math:`5/\sqrt(\mathrm{DECAM\_DEPTH})` gives flux in nanomaggies and :math:`-2.5[\log_{10}(5 / \sqrt(\mathrm{DECAM\_DEPTH})) - 9]` gives corresponding magnitude
DECAM_GALDEPTH              float32[6]   1/nanomaggy\ |sup2|   As for DECAM_DEPTH but for a galaxy (0.45" exp, round) detection sensitivity
=========================== ============ ===================== ===============================================

Mask Values
-----------

The DECAM_ANYMASK and DECAM_ALLMASK bit masks are defined as follows
from the CP Data Quality bits.

=== ===== =========================== ==================================================
Bit Value Name                        Description
=== ===== =========================== ==================================================
  0     1 detector bad pixel/no data  See the `CP Data Quality bit description`_.
  1     2 saturated                   See the `CP Data Quality bit description`_.
  2     4 interpolated                See the `CP Data Quality bit description`_.
  4    16 single exposure cosmic ray  See the `CP Data Quality bit description`_.
  6    64 bleed trail                 See the `CP Data Quality bit description`_.
  7   128 multi-exposure transient    See the `CP Data Quality bit description`_.
  8   256 edge                        See the `CP Data Quality bit description`_.
  9   512 edge2                       See the `CP Data Quality bit description`_.
 10  1024 longthin                    :math:`\gt 5\sigma` connected components with major axis :math:`\gt 200` pixels and major/minor axis :math:`\gt 0.1`.  To mask, *e.g.*, satellite trails.
=== ===== =========================== ==================================================

.. _`CP Data Quality bit description`: https://legacy.noirlab.edu/noao/staff/fvaldes/CPDocPrelim/PL201_3.html

Goodness-of-Fits
----------------

The DCHISQ values represent the |chi|\ |sup2| sum of all pixels in the source's blob
for various models.  This 5-element vector contains the |chi|\ |sup2| difference between
the best-fit point source (type="PSF"), simple galaxy model ("SIMP"),
de Vaucouleurs model ("DEV"), exponential model ("EXP"), and a composite model ("COMP"), in that order.
The "simple galaxy" model is an exponential galaxy with fixed shape of 0.45\ |Prime| and zero ellipticity (round)
and is meant to capture slightly-extended but low signal-to-noise objects.
The DCHISQ values are the |chi|\ |sup2| difference versus no source in this location---that is, it is the improvement from adding the given source to our model of the sky.  The first element (for PSF) corresponds to a traditional notion of detection significance.
Note that the DCHISQ values are negated so that positive values indicate better fits.
We penalize models with negative flux in a band by subtracting rather than adding its |chi|\ |sup2| improvement in that band.


The DECAM_RCHI2 values are interpreted as the reduced |chi|\ |sup2| pixel-weighted by the model fit,
computed as the following sum over pixels in the blob for each object:

.. math::
    \chi^2 = \frac{\sum \left[ \left(\mathrm{image} - \mathrm{model}\right)^2 \times \mathrm{model} \times \mathrm{inverse\, variance}\right]}{\sum \left[ \mathrm{model} \right]}

The above sum is over all images contributing to a particular filter.
The above can be negative-valued for sources that have a flux measured as negative in some bands
where they are not detected.

Galactic Extinction Coefficients
--------------------------------

The Galactic extinction values are derived from the SFD98 maps, but with updated coefficients to
convert E(B-V) to the extinction in each filter.  These are reported in linear units of transmission,
with 1 representing a fully transparent region of the Milky Way and 0 representing a fully opaque region.
The value can slightly exceed unity owing to noise in the SFD98 maps, although it is never below 0.

Extinction coefficients for the SDSS filters have been changed to the values recommended
by `Schlafly & Finkbeiner (2011; Table 4)`_ using the Fizpatrick 1999
extinction curve at R_V = 3.1 and their improved overall calibration of the SFD98 maps.
These coefficients are A / E(B-V) = 4.239,  3.303,  2.285,  1.698,  1.263 in ugriz,
which are different from those used in SDSS-I,II,III, but are the values used for SDSS-IV/eBOSS target selection.

Extinction coefficients for the DECam filters also use the `Schlafly & Finkbeiner (2011)`_ values,
with u-band computed using the same formulae and code at airmass 1.3 (Schlafly, priv. comm. decam-data list on 11/13/14).
These coefficients are :math:`A / E(B-V)` = 3.995, 3.214, 2.165, 1.592, 1.211, 1.064
for the DECam :math:`u`, :math:`g`, :math:`r`, :math:`i`, :math:`z`, :math:`Y` filters,
respectively. Note that these are *slightly* different from the coefficients in `Schlafly & Finkbeiner (2011)`_.

The coefficients for the four WISE filters are derived from Fitzpatrick 1999, as recommended by Schafly & Finkbeiner,
considered better than either the Cardelli et al 1989 curves or the newer Fitzpatrick & Massa 2009 NIR curve not vetted beyond 2 micron).
These coefficients are A / E(B-V) = 0.184,  0.113, 0.0241, 0.00910.

.. _`Schlafly & Finkbeiner 2011`: https://ui.adsabs.harvard.edu/abs/2011ApJ...737..103S/abstract
.. _`Schlafly & Finkbeiner (2011)`: https://ui.adsabs.harvard.edu/abs/2011ApJ...737..103S/abstract
.. _`Schlafly & Finkbeiner (2011; Table 4)`: https://ui.adsabs.harvard.edu/abs/2011ApJ...737..103S/abstract

Ellipticities
-------------

The ellipticity, |epsilon|, is different from the usual
eccentricity, :math:`e \equiv \sqrt{1 - (b/a)^2}`.  In gravitational lensing
studies, the ellipticity is taken to be a complex number:

.. math::

    \epsilon = \frac{a-b}{a+b} \exp( 2i\phi ) = \epsilon_1 + i \epsilon_2

Where |phi| is the position angle with a range of 180\ |deg|, due to the
ellipse's symmetry. Going between :math:`r, \epsilon_1, \epsilon_2`
and :math:`r, b/a, \phi`:

.. math::

    r           & = & r \\
    |\epsilon|  & = & \sqrt{\epsilon_1^2 + \epsilon_2^2} \\
    \frac{b}{a} & = & \frac{1 - |\epsilon|}{1 + |\epsilon|} \\
    \phi        & = & \frac{1}{2} \arctan \frac{\epsilon_2}{\epsilon_1} \\
    |\epsilon|  & = & \frac{1 - b/a}{1 + b/a} \\
    \epsilon_1  & = & |\epsilon| \cos(2 \phi) \\
    \epsilon_2  & = & |\epsilon| \sin(2 \phi) \\
