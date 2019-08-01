---
layout: default
title: JFilter Documentation
description: Version 1.0.15
---

[Main](../../../index.MD) > [1.0.15](../../index.MD) >  [Examples](../index.MD) > Example classes

# Example classes
This example classes used in examples

## User class
```java
public class User {
    private Integer id;
    private String email;
    private String fullName;
    private String password;
    private String secretKey;
    private Address address;

    public User() {
    }

    public Integer getId() {
        return id;
    }

    public User setId(Integer id) {
        this.id = id;
        return this;
    }

    public String getEmail() {
        return email;
    }

    public User setEmail(String email) {
        this.email = email;
        return this;
    }

    public String getFullName() {
        return fullName;
    }

    public User setFullName(String fullName) {
        this.fullName = fullName;
        return this;
    }

    public String getPassword() {
        return password;
    }

    public User setPassword(String password) {
        this.password = password;
        return this;
    }

    public String getSecretKey() {
        return secretKey;
    }

    public User setSecretKey(String secretKey) {
        this.secretKey = secretKey;
        return this;
    }

    public Address getAddress() {
        return address;
    }

    public User setAddress(Address address) {
        this.address = address;
        return this;
    }
}
```

## Address class
```java
public class Address {
    private Integer id;
    private Integer apartmentNumber;
    private Street street;

    public Address() {
    }

    public Integer getId() {
        return id;
    }

    public Address setId(Integer id) {
        this.id = id;
        return this;
    }

    public Integer getApartmentNumber() {
        return apartmentNumber;
    }

    public Address setApartmentNumber(Integer apartmentNumber) {
        this.apartmentNumber = apartmentNumber;
        return this;
    }

    public Street getStreet() {
        return street;
    }

    public Address setStreet(Street street) {
        this.street = street;
        return this;
    }
}
```

## Street class
```java
public class Street {
    private Integer id;
    private String streetName;
    private Integer streetNumber;

    public Street() {
    }

    public Integer getId() {
        return id;
    }

    public Street setId(Integer id) {
        this.id = id;
        return this;
    }

    public String getStreetName() {
        return streetName;
    }

    public Street setStreetName(String streetName) {
        this.streetName = streetName;
        return this;
    }

    public Integer getStreetNumber() {
        return streetNumber;
    }

    public Street setStreetNumber(Integer streetNumber) {
        this.streetNumber = streetNumber;
        return this;
    }
}
```
