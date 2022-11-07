# YAMCS developer guide


## Introduction 

Welcome to the YAMCS developer guide. This guide is created in order to explain and help you understand some of the core concepts you will use when developing the [OPS' YAMCS instance](https://gitlab.com/acubesat/ops/yamcs-instance)


### The [Integer Data Encoding Element](https://public.ccsds.org/Pubs/660x1g2.pdf#page=75)

Telemetered values that are encoded as integers are described with IntegerDataEncoding. A simple example showing its mostly used attributes is presented below: 

```
@encoding = Specifies integer numeric value to raw encoding method with the default being "unsigned"
@sizeInBits = The number of bits to use for the raw encoding with 8 being the default

<xtce:IntegerDataEncoding encoding="unsigned" sizeInBits="16" />
```

### The [Integer Parameter Type](https://public.ccsds.org/Pubs/660x1g2.pdf#page=125)

IntegerParameterType describes integer telemetry value data types if it is telemetered, or other local data types if the DataEncoding is not present (and the corresponding Parameter/ParameterProperties/dataSource is set). The IntegerParameterType is similar to scalars in construction overall. Below are two examples of IntegerParameterTypes:

- #### Declaring a 16bit unsigned IntegerParameterType
```
@name = The IntegerParameterType's name
@signed = Indicates whether the IntegerParameterType is signed or unsigned. The default is "true"

<xtce:IntegerArgumentType name="uint16_t" signed="false">
    <xtce:IntegerDataEncoding encoding="unsigned" sizeInBits="16" />
</xtce:IntegerArgumentType>
```

- #### Declaring the packet data length of the primary TM header
```
@name = the IntegerParameterType's name
@signed = Indicates whether the IntegerParameterType is signed or unsigned. The default is "true"
@initialValue = The IntegerParameterType's initial value in base 10 form

The UnitSet and Unit tags are optional. The UnitSet tag describes an ordered collection of units that form a unit-expression. The Unit tag describes the exponent, factor, form and description for a unit. In this case, the packet data length is measured in octets, as defined in the 7.4 section of the ECSS-Standard.

<xtce:IntegerParameterType name="packet_data_length" signed="false" initialValue="0">
    <xtce:UnitSet>
        <xtce:Unit description="Size">Octets</xtce:Unit>
    </xtce:UnitSet>
    <xtce:IntegerDataEncoding sizeInBits="16"></xtce:IntegerDataEncoding></xtce:IntegerParameterType>
```
Follow [this link](https://public.ccsds.org/Pubs/660x1g2.pdf#page=65) for more information regarding the UnitSet and Unit tags.


### [Float](https://public.ccsds.org/Pubs/660x1g2.pdf#page=56) and [Boolean](https://public.ccsds.org/Pubs/660x1g2.pdf#page=141) Parameter Types

The Float and Boolean parameter types are defined the same way as the IntegerParameterType.

- #### Declaring a FloatParameterType
```
The @sizeInBits in the FloatDataEncoding element is the number of bits to use for the raw encoding method with 32 being the default.

<xtce:FloatParameterType name="float_t">
   <xtce:FloatDataEncoding sizeInBits="32" />
</xtce:FloatParameterType>
```

- #### Declaring a BooleanParameterType
```
<xtce:BooleanParameterType name="bool_t">
   <xtce:IntegerDataEncoding sizeInBits="1" />
</xtce:BooleanParameterType>
```


### The [Enumerated Parameter Type](https://public.ccsds.org/Pubs/660x1g2.pdf#page=118)

The EnumeratedParameterType supports the description of enumerations, which are a list of values and their associated labels. Below is an example that demonstrates how an enumerated parameter type is declared and its mostly used attributes:

```
@name = The EnumeratedParameterType's name
@value = The enumeration element's value
@label = The enumeration element's label

<xtce:EnumeratedParameterType name="enumerated_parameter_type_example">
    <xtce:IntegerDataEncoding sizeInBits="16"></xtce:IntegerDataEncoding>
        <xtce:EnumerationList>
            <xtce:Enumeration value="0" label="label_1" />
            <xtce:Enumeration value="2" label="label_2" />
            <xtce:Enumeration value="4" label="label_3" />
            <xtce:Enumeration value="6" label="label_4" />
        </xtce:EnumerationList>
</xtce:EnumeratedParameterType>
```


### The [Absolute Time Parameter Type](https://public.ccsds.org/Pubs/660x1g2.pdf#page=146)

The AbsoluteTimeParameterType describes absolute time values and is defined as shown in the example below:

```
@name = The AbsoluteTimeParameterType's name
@offset = Linear intercept used as a shorter form specifying a calibrator to convert between the raw value and the engineering units
@scale = Linear slope used as a shorter form of specifying a calibrator to convert between the raw value and the engineering units 
<xtce:ReferenceTime> = Describes origin(epoch or reference) of this time type
<xtce:Epoch> = Epochs may be specified as an XS date where time is implied to be 00:00:00, xs dateTime, or string enumeration of common epochs. The enumerations are TAI(used by CCSDS and others), J2000, UNIX(also known as POSIX) and GPS

The example below is using UNIX as its reference time, whose count starts at January 1 1970 and is used by modern computers, linux systems etc. The offset and the scale are part of a linear transformation which has the form y = ax + b where "b" represents the offset, "a" represents the scale and "x" is the input. 

<xtce:AbsoluteTimeParameterType name="absolute_time_param_type_example">
     <xtce:Encoding offset="num_1" scale="num_2">
          <xtce:IntegerDataEncoding sizeInBits="32" />
     </xtce:Encoding>
     <xtce:ReferenceTime>
          <xtce:Epoch>UNIX</xtce:Epoch>
     </xtce:ReferenceTime>
</xtce:AbsoluteTimeParameterType>
```
