This project offers a simple API for translating [Google protocol buffer messages](http://code.google.com/p/protobuf/) to and from JSON text and [JsonCpp objects](http://jsoncpp.sourceforge.net/).

As an example, take the following protocol buffer definition:

```
package test;

message employee {
	required uint32 id = 1;
	required string first_name = 2;
	required string last_name = 3;
	optional string date_of_birth = 4;

	enum pay_type_enum {
	     hourly = 1;
	     salaried = 2;
	}

	optional pay_type_enum pay_type = 5;
	optional uint32 salary = 6;
	optional uint32 hourly_wage = 7;
	optional bool active = 8;
	repeated string departments = 9;
	optional uint64 hire_date = 10;
}
```

A protocol buffer message for the above definition can be initialized from JSON text.  The protocol buffer message can also generate JSON text based on its current state:

```
	const std::string message = "{\"active\":true,\"date_of_birth\":\"1/2/1970\",\"departments\":[\"dept1\",\"dept4\",\"dept9\"],\"first_name\":\"John\",\"hire_date\":1365109914,\"id\":12345,\"last_name\":\"Smith\",\"pay_type\":\"salaried\",\"salary\":1}\n";

	test::employee employee;
        // initialize protobuf message with JSON text
	json_protobuf::update_from_json(message, employee);

	ASSERT(employee.id() == 12345);
	ASSERT(employee.first_name() == "John");
	ASSERT(employee.last_name() == "Smith");
	ASSERT(employee.date_of_birth() == "1/2/1970");
	ASSERT(employee.pay_type() == json_protobuf::test::employee::salaried);
	ASSERT(employee.salary() == 1);
	ASSERT(employee.active());
	ASSERT(employee.hire_date() == 1365109914);

        std::string message2;
        // get the JSON text for this protobuf message
	json_protobuf::convert_to_json(employee, message2);
```

In addition, protocol buffer messages may be converted to and from JsonCpp objects:

```
        test::employee employee;
        // fill in protobuf message...
	
        Json::Value value;
        // convert protobuf message to JsonCpp object
	json_protobuf::convert_to_json(employee, value);

	test::employee employee2
        // convert JsonCpp object to protobuf message
        json_protobuf::update_from_json(value, employee2);
```

The library is built using the protocol buffer reflection API and requires only that the protobuf and JsonCpp libraries be available.