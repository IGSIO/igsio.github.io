### Mission
The IGSIO was founded after lenghty discussions regarding the standardization of tracked ultrasound communication to work towards improving the interoperability between industrial products and research software platforms [[1]](http://wiki.na-mic.org/Wiki/index.php/2016_Winter_Project_Week/Projects/TrackedUltrasoundStandardization). Prior to these discussions, the commonly accepted solution was to pass specially constructed messages over the OpenIGTLink protocol. This workaround solution indicated that a more formal definition was needed to enable the research community to move towards a vision held by the founding members.

The mission of the IGSIO is to develop a written standard defining the communication of tracked ultrasound systems between software platforms and to implement a reference implementation of an OpenIGTLink client and server for use by any adhering software platform.

### Goals
We seek to achieve the following goals:
* To advance the image-guided systems community through co-operation and collaboration
* To define a written standard for tracked ultrasound communication
* To provide a reference standard implentation of a tracked ultrasound client/server

### Founding Members
IGSIO was founded at the NAMIC 2016 Winter Project week in Cambridge, Massachusetts. Founding members include:
* Andras Lasso @lassoan ([Laboratory for Percutaneous Surgery](http://perk.cs.queensu.ca/), Queen's University, Canada)
* Adam Rankin @adamrankin ([VASST Laboratory](http://www.imaging.robarts.ca/petergrp/Research), Western University, Canada)
* Christian Askeland @christiana ([SINTEF Medical Technology](http://www.sintef.no/en/technology-and-society/departments/medical-technology/#/), Norway)
* Simon Drouin @drouin-simon ([Neuroimaging and Surgical Tools Lab](http://www.bic.mni.mcgill.ca/ResearchLabsIPL/HomePage), McGill University, Canada)
* Thomas Kirchner @thkirchner ([German Cancer Research Center (DKFZ)](https://www.dkfz.de/en/index.html), Helmholtz Association, Germany)
* Janek Gröhl @jgroehl ([German Cancer Research Center (DKFZ)](https://www.dkfz.de/en/index.html), Helmholtz Association, Germany)

### The Standard
The standard is presented here as a draft. Extensions to the OpenIGTLink protocol have been proposed [[2]](http://openigtlink.org/protocols/v3_proposal.html) and implementation of these extensions is underway [[3]](https://github.com/IGSIO/OpenIGTLinkIF).

> #### Standard Draft (r0.1)
> ##### Parameters
> These are the parameters that are common to the majority of ultrasound imaging devices and are thus supported by the standard. See individual commands for specific usage.

> *Read-write*
> * Depth (mm): (“40”, “45”, “50”, …) 
> * ImagingMode (string): (“bmode”, “bmode+rf”, “bmode+angio”, “rf+angio”, ...)
> * Probe (string): probe name (“L14-5/38”)
> * Frequency (MHz)
> * DynRange (db)
> * Gain (%)
> * Power (%)
> * Zoom (%)
> * SoundVelocity (m/s)
> * TGC (space-separated string, each entry -1.0 to 1.0)
> * AcquisitionState (string): “FREEZE” or “RUN”

> *Read-only*
> * ClipRectangleOrigin="27 27"
> * ClipRectangleSize="766 562"
> * FanAnglesDeg=”-30 30” (for 3D: FanAnglesDeg=”-30 30 -15 15”)
> * FanOriginPixel=”240 10”
> * FanRadiusStartPixel=”30”
> * FanRadiusStopPixel=”500”
> * Encoding: how to interpret values ("BRIGHTNESS", "RF_REAL", "RF_IQ_LINE", "RF_I_LINE_Q_LINE", "RGB_COLOR")

> ####Commands
> #####SetDeviceParameters
> Specify a list of parameters to set their current hardware values
> *Message*

    <Command>
        <Parameter Name=”Depth” Value=”45” />
        <Parameter Name=”Gain” Value=”35” />
    </Command>

> *Response 1*

    <Command>
      <Result>SetDeviceParameters: success</Result>
      <Parameter Name=”Depth” Value=”45” />
      <Parameter Name=”Gain” Value=”35” />
      ...
    </Command>

> *Response 2*

    <Command>
      <Result>SetDeviceParameters: failure</Result> <!-- see command status error code -->
    </Command>

> #####GetDeviceParameters
> Specify a list of parameters to retrieve their current hardware values (not cached)

> *Message*

    <Command>
      <Parameter Name=”Depth” />
      <Parameter Name=”Gain” />
      ...
    </Command>

> *Response*

    <Command>
      <Result>GetDeviceParameters: success</Result>
      <Parameter Name=”Depth” Value=”45” />
      <Parameter Name=”Gain” Value=”35” />
      ...
    </Command>

> #####SubscribeDeviceParameters
> This will cause the server to send a GetDeviceParameters reply every time any of the subscribed parameters change.

> *Message*

    <Command>
      <Parameter Name=”Depth” Subscribe=”TRUE” />
      <Parameter Name=”Gain” Subscribe=”FALSE” />
      ...
    </Command>

> *Response*

    <Command>
      <Result>SubscribeDeviceParameters: success</Result>
      <Parameter Name=”Depth” Subscribed="TRUE" />
      <Parameter Name=”Gain” Subscribed="FALSE" />
      ...
    </Command>

> #####GetCapabilities
> Request the functionality of the server's current configuration
> Note: future functionality support, SINTEF will specify and implement.

> *Message*

    <Command></Command>

> *Response*

    <Command>
      <Result>GetCapabilities: success</Result>
      <UltrasoundCapabilities>
        <Probes>
          <Probe name="L14-5/38"/>
          <Probe name="C5-2/42"/>
        </Probes>
        <ImagingModes>
          <ImagingMode Name=”b-mode+angio”>B-Mode,Angio</ImagingMode>
        </ImagingModes>
        <Streams>
          <Stream name="B-Mode">
            <Parameters>
              <Parameter name="depth" min="5" max="220" step="5"/>
            </Parameters>
          </Stream>
          <Stream name="Angio">
            <Parameters>
              <Parameter name="depth" min="5" max="220" step="5"/>
            </Parameters>
          </Stream>
        </Streams>
        <Presets>
          <Preset name="Vascular small object B+Angio">
            <Probe Name=”L14-5/38” />
            <Parameter Name=”Depth” Value=”50” />
          </Preset>
        </Presets>
      </UltrasoundCapabilities>
    </Command>

### Links
1. http://wiki.na-mic.org/Wiki/index.php/2016_Winter_Project_Week/Projects/TrackedUltrasoundStandardization
2. http://openigtlink.org/protocols/v3_proposal.html
3. https://github.com/IGSIO/OpenIGTLinkIF
