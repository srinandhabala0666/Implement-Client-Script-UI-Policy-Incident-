Implement Client Script & UI Policy – Incident

A ServiceNow micro project demonstrating how UI Policies and Client Scripts can be used to enforce data integrity, automate field values, control field behavior, and validate Incident records before submission.

📌 Project Overview

Incident records require consistent and accurate information for effective triage, assignment, SLA management, reporting, and resolution.

This project implements client-side controls on the Incident table to:

Control field behavior based on Incident Impact.

Automatically set Urgency for high-impact Incidents.

Make fields mandatory under specific conditions.

Prevent invalid Incident submissions.

Restrict direct State changes through list editing.

Demonstrate dynamic UI behavior using UI Policies and Client Scripts.

🎯 Objective

The objective of this project is to demonstrate practical implementation of ServiceNow client-side functionality using:

UI Policies

UI Policy Actions

onChange Client Scripts

onSubmit Client Scripts

onCellEdit Client Scripts

Form validation

The configuration ensures that Incident records contain the required information and follow predefined business rules before they are submitted.

🛠️ Technologies & Skills

Platform: ServiceNow

Module: Incident Management

Configuration: UI Policy, UI Policy Actions

Scripting: JavaScript

Client Scripts: onChange, onSubmit, onCellEdit

Validation: Client-side form validation

📋 Project Requirements

The following configurations are implemented on the Incident table.

1. UI Policy – High Impact Control

Name: High Impact Control

Table: Incident

Condition:

Field	Operator	Value
Impact	is	1 – High

Configuration:

Active: true

Reverse if false: true

Assignment Group: Mandatory

When Impact is set to High, the UI Policy applies the configured field behavior. When the condition becomes false, the UI Policy reverses the changes.

2. UI Policy Action – Urgency

A UI Policy Action is created under High Impact Control.

Configuration	Value
Field	Urgency
Read-only	True
Visible	Unchanged

When Impact is High, the Urgency field becomes read-only.

3. onChange Client Script – Auto Set Urgency

Name: Auto set urgency for high impact

Table: Incident

Type: onChange

Field: Impact

This script automatically sets Urgency to High when Impact changes to High.

function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue == '') {
        return;
    }

    if (newValue == '1') {
        g_form.setValue('urgency', '1');
        g_form.addInfoMessage(
            'Urgency set to High for High impact incident.'
        );
    }
}

4. onSubmit Client Script – Assigned To Validation

Name: Prevent save if Assigned To missing

Table: Incident

Type: onSubmit

This script prevents a High Impact Incident from being saved when Assigned To is empty.

function onSubmit() {
    if (g_form.getValue('impact') == '1' &&
        g_form.getValue('assigned_to') == '') {

        g_form.showErrorBox(
            'assigned_to',
            'Assigned To is mandatory for High impact incidents.'
        );

        return false;
    }

    return true;
}

5. onCellEdit Client Script – Prevent State List Editing

Name: Prevent state change via list edit

Table: Incident

Type: onCellEdit

Field: State

This Client Script prevents users from changing the Incident State directly from a list view.

function onCellEdit(sysIDs, table, oldValues, newValue, callback) {

    alert(
        'State cannot be updated using list editing. Please open the Incident.'
    );

    callback(false);
}


State changes are still allowed when the Incident is opened and updated through the form.

🧪 Testing
Test 1 – Mandatory Enforcement

Navigate to Incident → Create New.

Set Impact to High.

Leave Assigned To empty.

Click Submit.

Verify that the Incident is not saved.

Verify that an error is displayed for Assigned To.

Test 2 – Successful Save

Set Impact to High.

Select a user in Assigned To.

Verify that Urgency is automatically set to High.

Verify that Urgency is read-only.

Click Submit.

Confirm that the Incident saves successfully.

Test 3 – Reverse Condition

Open an Incident with Impact = High.

Change Impact to Medium.

Verify that the UI Policy is reversed.

Confirm that Urgency becomes editable.

Confirm that fields controlled by the policy return to their normal behavior.

Save the Incident.

Test 4 – List Edit Blocking

Navigate to Incident → All.

Locate an Incident.

Attempt to edit the State field directly from the list.

Verify that the alert appears.

Confirm that the State value remains unchanged.

Test 5 – Form-Based State Update

Open an Incident.

Change the State field from the Incident form.

Click Update.

Confirm that the State change is saved successfully.

📊 Expected Behavior
Scenario	Expected Result
Impact = High	High Impact UI Policy is triggered
Impact = High	Urgency automatically becomes High
Impact = High	Urgency becomes read-only
Impact = High + Assigned To empty	Record cannot be submitted
Impact changed from High	UI Policy behavior is reversed
State edited from list	Update is blocked
State edited from form	Update is allowed
📁 Project Structure
Implement-Client-Script-UI-Policy-Incident/
│
├── README.md
│
├── Client-Scripts/
│   ├── Auto-set-urgency-onChange.js
│   ├── Prevent-save-assigned-to-onSubmit.js
│   └── Prevent-state-list-edit-onCellEdit.js
│
├── UI-Policies/
│   └── High-Impact-Control.md
│
└── Screenshots/
    ├── ui-policy.png
    ├── ui-policy-action.png
    ├── onchange-script.png
    ├── onsubmit-script.png
    ├── oncelledit-script.png
    └── testing.png

🔑 Key Learning Outcomes

Through this project, the following ServiceNow concepts are demonstrated:

Creating and configuring UI Policies.

Creating UI Policy Actions.

Dynamically controlling form fields.

Using g_form APIs in Client Scripts.

Implementing onChange automation.

Implementing onSubmit validation.

Preventing invalid record submissions.

Controlling list-edit behavior with onCellEdit.

Testing client-side business rules.

Understanding the interaction between UI Policies and Client Scripts.

⚠️ Important Note

Client-side controls primarily improve form usability and user experience. They should not be considered the only layer of data protection for critical business rules. For requirements that must be enforced regardless of how records are created or updated, consider appropriate server-side validation, such as Business Rules or Data Policies.

🏁 Conclusion

This project demonstrates how ServiceNow UI Policies and Client Scripts can work together to improve Incident data quality and enforce conditional business rules.

The implementation provides dynamic field behavior, automated Urgency assignment, save-time validation, and list-edit restrictions while keeping the solution lightweight and easy to maintain.

Project: Implement Client Script & UI Policy – Incident
Platform: ServiceNow
Table: Incident
