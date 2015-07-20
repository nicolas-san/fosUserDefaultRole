# fosUserDefaultRole
Simple listener on REGISTRATION_SUCCESS event to add a role by default to your user

Why ROLE_USER can not be used as default role in a listener !

yourProjectRoot/vendor/friendsofsymfony/user-bundle/Model/UserInterface.php :

interface UserInterface extends AdvancedUserInterface, \Serializable
{
    const ROLE_DEFAULT = 'ROLE_USER';
    const ROLE_SUPER_ADMIN = 'ROLE_SUPER_ADMIN';

Fos user bundle default static role

In :

yourProjectRoot/vendor/friendsofsymfony/user-bundle/Model/User.php

 

public function addRole($role)
    {
        $role = strtoupper($role);
        if ($role === static::ROLE_DEFAULT) {
            return $this;
        }

        if (!in_array($role, $this->roles, true)) {
            $this->roles[] = $role;
        }

        return $this;
    }

(line of code from FOS userBundle repo on github)

If you try to add ROLE_USER, as ROLE_USER is ROLE_DEFAULT, you role is not added to $this->roles[] and not persisted, it is why even with a good working listener, ROLE_USER is never write into user account details in database.
