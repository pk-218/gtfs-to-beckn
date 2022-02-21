# How discovery works

## Communication Flow 

![KOMN Sequence Diagrams-Discovery](https://user-images.githubusercontent.com/65583767/154055003-56a5d978-a7b2-4cdc-9c11-bdb06dcc9b36.png)

The Beckn Application Platform (BAP) will send the search intent to the Beckn Gateway (BG) using the `/search` call. The BG will broadcast the search intent to all the Beckn Provider Platforms (BPPs) in the network for the domain. Each BPP can choose to send back its catalog based on its business logic using the `/on_search` call to the BG. The BG will forward the catalog to the BAP.
  
## Search Request
  
### Request

#### Search using start and end station codes

```
{
    "context": {
        "domain": "nic2004:60212",
        "country": "IND",
        "city": "Kochi",
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
                        "station_code": "ST04"
                    }
                }
            }
        }
    }
}
```

Here the user wants tickets between stations with station code `ST01` and `ST04`. So the station code of the origin station `ST01` is given in `message.intent.fulfillment.start.location.station_code` and the station code of the destination station `ST04` is given in `message.intent.fulfillment.end.location.station_code`

#### Search using start and end location GPS

```
{
    "context": {
        "domain": "nic2004:60212",
        "country": "IND",
        "city": "Kochi",
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
                        "gps": "10.021401, 76.306693"
                    }
                },
                "end": {
                    "location": {
                        "gps": "10.109684, 76.350800"
                    }
                }
            }
        }
    }
}
```

Here the user wants travel between 2 locations represented by GPS co-ordinates. So the GPS co-ordinates of from where the user starts their journey `10.021401, 76.306693` is given in `message.intent.fulfillment.start.location.gps` and the GPS co-ordinates at where the user ends their journey `10.109684, 76.350800` is given in `message.intent.fulfillment.end.location.gps`. 
If `ENABLE_LOCATION_SERVICES` is set as true in `./config/config.ts` the BPP will get stations closest to the locations from the GTFS data to determine the start and end stops. Multiple stations can be returned as well in case where there are multiple stations at comparable distances from a GPS co-ordinate. 
If `ENABLE_LOCATION_SERVICES` is set as false in `./config/config.ts` then the BPP will send the info of all the stops in the GTFS data as response.

### Response
  
```
{
    "context": {
        "domain": "nic2004:60212",
        "country": "IND",
        "city": "",
        "action": "on_search",
        "core_version": "0.9.3",
        "bap_id": "test-bap",
        "bap_uri": "https://test-bap.com",
        "transaction_id": "12391830183",
        "message_id": "2131030183",
        "timestamp": "2022-02-12T09:55:41.161Z",
        "bpp_id": "transit-bpp",
        "bpp_uri": "https://transit-bpp.com/"
    },
    "message": {
        "catalog": {
            "bpp/descriptor": {
                "name": "BPP"
            },
            "bpp/providers": [
                {
                    "id": "abc-transit",
                    "descriptor": {
                        "name": "ABC Transit Rail Limited"
                    },
                    "locations": [
                        {
                            "id": "ST10",
                            "descriptor": {
                                "name": "Station 10"
                            },
                            "station_code": "ST10",
                            "gps": "10.0152,76.3023"
                        },
                        {
                            "id": "ST01",
                            "descriptor": {
                                "name": "Station 1"
                            },
                            "station_code": "ST01",
                            "gps": "10.1099,76.3495"
                        },
                        {
                            "id": "ST09",
                            "descriptor": {
                                "name": "Station 9"
                            },
                            "station_code": "ST09",
                            "gps": "10.0251,76.3083"
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
                                "value": "40"
                            },
                            "location_id": "ST10",
                            "fulfillment_id": "ST10_TO_ST01",
                            "matched": true
                        },
                        {
                            "id": "sjt",
                            "descriptor": {
                                "name": "Single Journey Ticket",
                                "code": "SJT"
                            },
                            "price": {
                                "currency": "INR",
                                "value": "35"
                            },
                            "location_id": "ST09",
                            "fulfillment_id": "ST09_TO_ST01",
                            "matched": true
                        }
                    ],
                    "fulfillments": [
                        {
                            "id": "ST10_TO_ST01",
                            "start": {
                                "location": {
                                    "id": "ST10"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T02:48:30.000Z"
                                }
                            },
                            "end": {
                                "location": {
                                    "id": "ST01"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T03:20:00.000Z"
                                }
                            }
                        },
                        {
                            "id": "ST10_TO_ST01",
                            "start": {
                                "location": {
                                    "id": "ST10"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T02:52:36.000Z"
                                }
                            },
                            "end": {
                                "location": {
                                    "id": "ST01"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T02:30:00.000Z"
                                }
                            }
                        },
                        {
                            "id": "ST10_TO_ST01",
                            "start": {
                                "location": {
                                    "id": "ST10"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T03:02:12.000Z"
                                }
                            },
                            "end": {
                                "location": {
                                    "id": "ST01"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T03:24:44.000Z"
                                }
                            }
                        },
                        {
                            "id": "ST09_TO_ST01",
                            "start": {
                                "location": {
                                    "id": "ST09"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T02:50:10.000Z"
                                }
                            },
                            "end": {
                                "location": {
                                    "id": "ST01"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T02:30:00.000Z"
                                }
                            }
                        },
                        {
                            "id": "ST09_TO_ST01",
                            "start": {
                                "location": {
                                    "id": "ST09"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T02:51:25.000Z"
                                }
                            },
                            "end": {
                                "location": {
                                    "id": "ST01"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T03:20:00.000Z"
                                }
                            }
                        },
                        {
                            "id": "ST09_TO_ST01",
                            "start": {
                                "location": {
                                    "id": "ST09"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T03:04:47.000Z"
                                }
                            },
                            "end": {
                                "location": {
                                    "id": "ST01"
                                },
                                "time": {
                                    "timestamp": "2022-02-13T03:24:44.000Z"
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

The above shows a sample response where 2 tickets are represented. One is for `Station 10 to Station 1` and the second is for `Station 9 to Station 1`. All the stations referred in the catalog are present in `locations` array in `message.catalog.bpp/providers`. Details like name of the station, its GPS co-ordinate, station code etc can be found here.
Both of the tickets represented here are single journey tickets and hence have the same `item_id`. These can be seen in the `items` array in `message.catalog.bpp/providers`. The `location_id` refers to the `id` in the `locations` array and represents the origin station. The fare of the tickets can be found in the `price` object here.
The `fulfillment_id` of the items in the items array are `ST10_TO_ST01` and `ST09_TO_ST01` which refers to objects in the fulfillments array in `message.catalog.bpp/providers`. For each object in the fulfillments array, there will be a `start` and `end` object. Here `start.location.id` will is used to represent to starting station of the trip and will refer to an `id` in the locations array. Similarly `end.location.id` will is used to represent to destination station of the trip and will also refer to an `id` in the same locations array. 
There can be multiple trips between the same stations. This is why there are multiple objects in the `fulfillments` array with the same `start.location` and `end.location` but different `start.time.timestamp` and `end.time.timestamp`. If we take the example of fulfillment_id `ST10_TO_ST01` from above, this represents trips from Station 10 to Station 1. Hence for all objects with this id in the fulfillments array, the `start.location.id` is `ST10` and `end.location.id` is `ST01`. You will be able to see that it has different `start.time.timestamp` and `end.time.timestamp` values.

In cases where `ENABLE_LOCATION_SERVICES` is set as false in `./config/config.ts` then the BPP can send a response as below :

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
        "transaction_id": "0992ae6c-ac36-4cb8-b0cf-f4c5cca52103",
        "message_id": "a99624ac-8137-45a5-89af-bfd846a8e123",
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
                            "id": "ST02",
                            "descriptor": {
                                "name": "Station 2"
                            },
                            "station_code": "ST02",
                            "gps": "10.0951,76.3466"
                        },
                        {
                            "id": "ST03",
                            "descriptor": {
                                "name": "Station 3"
                            },
                            "station_code": "ST03",
                            "gps": "10.0873,76.3428"
                        },
                        {
                            "id": "ST04",
                            "descriptor": {
                                "name": "Station 4"
                            },
                            "station_code": "ST04",
                            "gps": "10.0793,76.3389"
                        },
                        {
                            "id": "ST05",
                            "descriptor": {
                                "name": "Station 5"
                            },
                            "station_code": "ST05",
                            "gps": "10.0727,76.3336"
                        },
                        {
                            "id": "ST06",
                            "descriptor": {
                                "name": "Station 6"
                            },
                            "station_code": "ST06",
                            "gps": "10.0586,76.322"
                        },
                        {
                            "id": "ST07",
                            "descriptor": {
                                "name": "Station 7"
                            },
                            "station_code": "ST07",
                            "gps": "10.0467,76.3182"
                        },
                        {
                            "id": "ST08",
                            "descriptor": {
                                "name": "Station 8"
                            },
                            "station_code": "ST08",
                            "gps": "10.0361,76.3144"
                        },
                        {
                            "id": "ST09",
                            "descriptor": {
                                "name": "Station 9"
                            },
                            "station_code": "ST09",
                            "gps": "10.0251,76.3083"
                        },
                        {
                            "id": "ST10",
                            "descriptor": {
                                "name": "Station 10"
                            },
                            "station_code": "ST10",
                            "gps": "10.0152,76.3023"
                        },
                        {
                            "id": "ST11",
                            "descriptor": {
                                "name": "Station 11"
                            },
                            "station_code": "ST11",
                            "gps": "10.0064,76.3048"
                        },
                        {
                            "id": "ST12",
                            "descriptor": {
                                "name": "Station 12"
                            },
                            "station_code": "ST12",
                            "gps": "10.0002,76.2989"
                        },
                        {
                            "id": "ST13",
                            "descriptor": {
                                "name": "Station 13"
                            },
                            "station_code": "ST13",
                            "gps": "9.9943,76.2914"
                        },
                        {
                            "id": "ST14",
                            "descriptor": {
                                "name": "Station 14"
                            },
                            "station_code": "ST14",
                            "gps": "9.9914,76.2884"
                        },
                        {
                            "id": "ST15",
                            "descriptor": {
                                "name": "Station 15"
                            },
                            "station_code": "ST15",
                            "gps": "9.9834,76.2823"
                        },
                        {
                            "id": "ST16",
                            "descriptor": {
                                "name": "Station 16"
                            },
                            "station_code": "ST16",
                            "gps": "9.9732,76.2851"
                        }
                    ],
                    "items": [
                        {
                            "id": "sjt",
                            "descriptor": {
                                "name": "Single Journey Ticket",
                                "code": "SJT"
                            }
                        }
                    ]
                }
            ]
        }
    }
}
```

Here the BPP has just returned all the stop information from GTFS in the locations array.

Exact mappings from GTFS can be found [here](./GTFS-TO-BECKN.md)