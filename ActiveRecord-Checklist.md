* Are there unnecessary n+1 queries?
* Are there missing indexes?
* Defined #to_s?
* Validate associations (e.g. validates_associated :books) required?
* Specify dependencies if foreign keys are not defined (e.g. has_many dependent: destroy)
* All attribute validations?
* Null constraints?
* Default attributes?
* Foreign keys?
* Conforms to the [[ActiveRecord Style Guide]]?