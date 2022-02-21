## Search

### Search by station codes
*Here the BPP will take origin and destination stop_id from start and end station_codes*

```
{
    "context": {
        "domain": "nic2004:60212",
        "country": "IND",
        "city": "",
        "action": "search",
        "core_version": "0.9.3",
        "bap_id": "test-bap",
        "bap_uri": "https://test-bap.com",
        "transaction_id": "12391830183",
        "message_id": "2131030183",
        "timestamp": "2022-02-12T09:55:41.161Z"
    },
    "message": {
        "intent": {
            "fulfillment": {
                "start": {
                    "location": {
                        "station_code": "ST01"
                    }
                },
                "end": {
                    "location": {
                        "station_code": "ST05"
                    }
                }
            }
        }
    }
}
```

- `message.intent.fulfillment.start.location.station_code` : **stops.txt** >> stop_id
- `message.intent.fulfillment.end.location.station_code` : **stops.txt** >> stop_id

### Search by GPS co-ordinates

*Here the BPP can choose to find the closest stops from start and end GPS co-ordinates or just send a list of all its stops*

```
{
    "context": {
        "domain": "nic2004:60212",
        "country": "IND",
        "city": "",
        "action": "search",
        "core_version": "0.9.3",
        "bap_id": "test-bap",
        "bap_uri": "https://test-bap.com",
        "transaction_id": "12391830183",
        "message_id": "2131030183",
        "timestamp": "2022-02-12T09:55:41.161Z"
    },
    "message": {
        "intent": {
            "fulfillment": {
                "start": {
                    "location": {
                        "gps": "10.021401,76.306693"
                    }
                },
                "end": {
                    "location": {
                        "gps": "10.109684,76.350600"
                    }
                }
            }
        }
    }
}
```
- `message.intent.fulfillment.start.location.gps` : **stops.txt** >> stop_lat, stop_lon
- `message.intent.fulfillment.end.location.gps` : **stops.txt** >> stop_lat, stop_lon

## On_search

```
{
    "context": {
        "domain": "nic2004:60212",
        "country": "IND",
        "city": "Kochi",
        "action": "on_search",
        "core_version": "0.9.3",
        "bpp_id": "test.bpp.transit.com",
        "bpp_uri": "https://test.bpp.transit.com/",
        "bap_id": "test.bap.mobility.com",
        "bap_uri": "https://test.bap.mobility.com/protocol/",
        "transaction_id": "0992ae6c-ac36-4cb8-b0cf-f4c5cca50103",
        "message_id": "a99624ac-8137-45a5-89af-bfd846a8ec23",
        "timestamp": "2022-02-16T17:28:28.867Z"
    },
    "message": {
        "catalog": {
            "bpp/descriptor": {
                "name": "BPP"
            },
            "bpp/providers": [
                {
                    "id": "abc_transit",
                    "descriptor": {
                        "name": "ABC Transit Rail Limited"
                    },
                    "locations": [
                        {
                            "id": "ST01",
                            "descriptor": {
                                "name": "Station 1"
                            },
                            "station_code": "ST01",
                            "gps": "10.1099,76.3495"
                        },
                        {
                            "id": "ST04",
                            "descriptor": {
                                "name": "Station 4"
                            },
                            "station_code": "ST04",
                            "gps": "10.0793,76.3389"
                        }
                    ],
                    "items": [
                        {
                            "id": "sjt",
                            "descriptor": {
                                "name": "Single Journey Ticket",
                                "code": "SJT"
                            },
                            "price": {
                                "currency": "INR",
                                "value": "20"
                            },
                            "location_id": "ST01",
                            "fulfillment_id": "ST01_TO_ST04",
                            "matched": true
                        }
                    ],
                    "fulfillments": [
                        {
                            "id": "ST01_TO_ST04",
                            "start": {
                                "location": {
                                    "id": "ST01"
                                },
                                "time": {
                                    "timestamp": "2022-02-16T00:26:36.000Z"
                                }
                            },
                            "end": {
                                "location": {
                                    "id": "ST04"
                                },
                                "time": {
                                    "timestamp": "2022-02-16T00:36:05.000Z"
                                }
                            }
                        },
                        {
                            "id": "ST01_TO_ST04",
                            "start": {
                                "location": {
                                    "id": "ST01"
                                },
                                "time": {
                                    "timestamp": "2022-02-16T01:36:46.000Z"
                                }
                            },
                            "end": {
                                "location": {
                                    "id": "ST04"
                                },
                                "time": {
                                    "timestamp": "2022-02-16T01:46:05.000Z"
                                }
                            }
                        },
                        {
                            "id": "ST01_TO_ST04",
                            "start": {
                                "location": {
                                    "id": "ST01"
                                },
                                "time": {
                                    "timestamp": "2022-02-16T02:51:55.000Z"
                                }
                            },
                            "end": {
                                "location": {
                                    "id": "ST04"
                                },
                                "time": {
                                    "timestamp": "2022-02-16T03:01:45.000Z"
                                }
                            }
                        }
                    ]
                }
            ]
        }
    }
}
```

### Provider details

- `message.catalog.bpp/providers.id` : **agency.txt** >> agency_id
- `message.catalog.bpp/providers.descriptor.name` : **agency.txt** >> agency_name

### Location details

- `message.catalog.bpp/providers.locations.id` : **stops.txt** >> stop_id
- `message.catalog.bpp/providers.locations.descriptor.name` : **stops.txt** >> stop_name
- `message.catalog.bpp/providers.locations.station_code` : **stops.txt** >> stop_id
- `message.catalog.bpp/providers.locations.gps` : **stops.txt** >> stop_lat, stop_lon

### Item details

- `message.catalog.bpp/providers.items.id` : ID of the fare product being sold
- `message.catalog.bpp/providers.items.descriptor.name` : Name of the fare product being sold (Single journey ticket, Monthly pass etc)
- `message.catalog.bpp/providers.items.descriptor.code` : Code of the fare product being sold
- `message.catalog.bpp/providers.items.fulfillment_id` : ID of fulfillment that is applicable to this fare product (refers to `message.catalog.bpp/providers.fulfillments.id`)

#### In case of single journey tickets
*Single Journey tickets will be for an origin and destination stop. Here the fare_id needs to be mapped from **fare_rules.txt** using origin_id and destination_id as the stop_id for origin and destination stop)*
- `message.catalog.bpp/providers.items.price.currency` : **fare_attributes.txt** >> currency_type
- `message.catalog.bpp/providers.items.price.value` : **fare_attributes.txt** >> price
- `message.catalog.bpp/providers.items.location_id` : **stops.txt** >> origin stop's stop_id (refers to `message.catalog.bpp/providers.locations.id`)

### Fulfillment details

- `message.catalog.bpp/providers.fulfillments.id` : ID of fulfillmenet. This is used to link this fulfillment to a fare product defined in Items

#### In case of single journey tickets
*Single Journey tickets will be for an origin and destination stop. Here in **stop_times.txt** using origin_id and destination_id as the stop_id for origin and destination stop, the BPP will find all the possible trips for the scheduled service)*

- `message.catalog.bpp/providers.fulfillments.start.location.id` : Origin stop code (refers to `message.catalog.bpp/providers.locations.id`)
- `message.catalog.bpp/providers.fulfillments.start.time.timestamp` : **stop_times.txt** >> departure_time
- `message.catalog.bpp/providers.fulfillments.end.location.id` : Destination stop code (refers to `message.catalog.bpp/providers.locations.id`)
- `message.catalog.bpp/providers.fulfillments.end.time.timestamp` : **stop_times.txt** >> departure_time