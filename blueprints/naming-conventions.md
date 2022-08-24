# Naming Conventions

When creating a subclass of uk.gov.homeoffice.digital.sas.jparest.models.BaseEntity One has to define the name of the
RESTful resource and the name of the Database table that represents the new entity.

# RESTful Resources
RESTful resources will be defined using the @Resource annotation. These resources should use kebab case as the naming
convention. I.E

`@Resource(path = "time-period-type")`

# Database Table
The database table resources will be defined using the @Entity annotation. These resources should use snake-case as the 
naming convention. I.E 

`@Entity(name = "time_period-type")`