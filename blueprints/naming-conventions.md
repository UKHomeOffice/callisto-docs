# Naming Conventions

When creating a subclass of uk.gov.homeoffice.digital.sas.jparest.models.BaseEntity One has to define the name of the
RESTful resource and the name of the Database table that represents the new entity.

# RESTful Resources
RESTful resources will be defined using the @Resource annotation. These resources should use kebab case as the naming
convention and the name of the endpoints should be plural name. e.g.

`@Resource(path = "time-period-types")`

# Database Table
The database table resources will be defined using the @Entity annotation. These resources should use snake-case as the 
naming convention. Please note that the table name should not be plural name. I.E.

`@Entity(name = "time_period_type")`
