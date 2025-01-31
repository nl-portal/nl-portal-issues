# Feature: Configuration Panel

## Summary 

A configuration panel to configure functionality of the NL Portal.

## Motivation

It should be possible to adjust configurations of the NL Portal in the configuration panel. To allow for easier management and control of configurations without the need for a development team 
interference. As currently all configurations of the NL Portal happen via application properties and/or environment variables, which is not immediate and require redeployments. This 
creates a relience of CI/CD management to change basic configurations. With this feature we would like to improve the ability of managing configurations within NL Portal and improve its
usability in production. 

The goal of the feature is to create an Configuration Panel that is a standalone application in which configurations can be configured without having to change configurations in application 
properties or environment variables, and prevent unnecessary restarts of the NL Portal due to configuration changes.

## Requirements

- Standalone application (single instance)
- All configurations should still be configurable through application properties and environment variables
- Configurations set through the connfiguration panel are leading and complete
- Changes to configurations should not require a reboot of the NL Portal application
- Configuration panel has it's own authentication, and anyone with access can do anything

## Architecture

The configuration panel has access to the same database as the NL Portal, however it has both read and write access to the configuration tables (nlp_conf_*). The NL Portal only has read access to 
the configuration tables. A cache was added between the NL Portal and the database for accessing the configuration tables as configurations change infrequently and reading the configurations happens 
frequently.

![Untitled-2023-12-06-1546](https://github.com/user-attachments/assets/05e029eb-1d42-4d84-ad0c-f12b416ae729)

## Technology Stack

- Kotlin
- Spring Boot
- React

## Rationale and alternatives

- Why is this design the best in the space of possible designs?
  - It enables more customers/users who otherwise would not be able to setup and use the NL Portal
- What other designs have been considered and what is the rationale for not choosing them?
  - We have considered not creating a configuration application. This would mean configurating the NL Portal would be a developer / devops tasks and could not be done by a more everyday user.
- What objections immediately spring to mind? How have you addressed them?
  - It adds complexity.
    - We need to be able to adjust connection details while the application is running.
    - We need to be able to enable/disable components at runtime.  
  - It does not add value to existing users.
    - The current users are more advanced users that might not need the ability of the configuration application. However the focus of the NL Portal goes beyond advanced users.
- What is the impact of not doing this?
  - It means that configuration of the NL Portal stays part of the CI/CD pipeline and requires a DevOps engineer or developer to adjust configurations. Not allowing users to update configurations
    as they see fit.

## Unresolved Questions

- n/a

## Future Possibilities

- Add more customization of the NL Portal to the configuration application (e.g. themas, styling, enable/disbale features, translations).
- Add a more CMS like functionality (page composition, customizable footers).
- Incident handling
