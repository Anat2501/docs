# Variable Length Fields

Variable length fields are used when we wish to send data of undetermined length. Rather than having a separate field for the length of these data, data types were devised which include these fields.

These structures were designed so that a reader can determine their length before reading the data, so that the data could be appropriately handled as it is read from a stream with no lookbacks.

