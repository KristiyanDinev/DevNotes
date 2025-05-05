# Entity Framework Core (EF Core)

## Relationships

If you want one to many:

one class has a collection and the other has a reference

```
User
ICollection<Roles> Roles;
```

```
Role

int UserId;
User User;
```

Like: One Role has one user and one user can have multiple roles. This got to be implemented.


-----
UserRoleModel - The assignment of the roles to the user / the relationship between the user and his roles

RoleModel - The actual role

RolePermissionModel - Which services that role can access. Just the permissions of the role.

ServiceModel - The service and for navigation a collection of permissions.

-----

