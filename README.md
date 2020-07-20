Inspired by this article https://www.salesforceben.com/how-to-build-a-client-side-data-loader-app/  I thought I'd see what this application would look like in LWC, since I've never used VisualForce before.

### Requirements:

Create an app which allows the user to upload a csv, display the contents of the file, and provide the ability to save the information to a specific sobject.

## file upload

There is a file upload component in the LWC namespace, but it won't be suitable for the task as it requires a record Id, and be on a record page.

Our component needs to be usable anywhere, for any sobject.

What we'll use instead is the lightning input of type file.

    <lightning-input
        label="Upload csv"
        type="file"
        onlick={handleUploadFile}>
    </lightning-input>

In our JavaScript file we need to add a function which handles the button click.

The code needs to check that the file type is csv, and display an error if not.

Once we have the csv contents we'll use the same csv parser as Andy. To use this in the component, we first need to create a static resource which can be referenced.

We need to import the loadScript method from the standard library, and load the Papaparse JavaScript in the rendered callback within a promise.

In the method which handles the contents of the csv, fire the Papaparse with the data, specifying a callback when the parse has completed.

The display is going to use a lightning data table, which needs a specific format of data.

To do this manipulation, create a new JavaScript library that can take care of diverting the Papaparse structure to the data table structure.

This library can handle data that has a header row or not, but we're going to assume that the user has added a header row containing the sobject field names or API names that can be matched against the selected sobject.

Before writing the implementation, how will it be tested?

If we pass X into the data converter, can we be sure we get Y returned and not Z?

We need a test file.

Which test cases do we need to test?

### happy path
- pass good data which has header info
- pass good data which does not have a header

### unhappy path?

We're getting data which has come directly from the Papaparse parse call, so if the were any errors they would already have been caught.

Luckily, LWC comes complete with a JavaScript testing framework called Jest.



## selected sobject

How can we insert data without knowing which object to insert it into?

We need some way of choosing an sobject.

A lightning combo box could do the job, but depending on how many sobjects your org has, it could be a very long list, making it a problem for the browser to render.

For simplicity, let's just add a text box with the API name of the object