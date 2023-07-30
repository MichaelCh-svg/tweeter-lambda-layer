# Tweeter Lambda Layer

This lambda layer will be needed for each of the 14 lambdas in aws for the tweeter project.

Since node dependencies are not compiled to javascript during the 'tsc' compile command, 
the node dependences have to be uploaded to aws lamba separately. 

Follow these steps to setup the lambda layer.

1. Compile a module containing the project's entities and web request/response objects. 
    - Instructions are at the following link: https://github.com/MichaelCh-svg/tweeter-entities-chemps-svg
    - We compile this dependency as a module so that we can reuse the same code for the client and the server.
2. Link the previously compiled module into this project. Run 'npm link --save [project folder path]'.
3. Run 'npm update' to update and install the dependencies.
    - To install the dependencies without updating them, run 'npm i'.
            - Occasionally updates cause compatibility conflicts, causing a need to revert to an old dependency to get the project running before finding a fix.
    - To later want to update the dependencies, remove the node_modules folder so that all files will be up to date (not just the ones that get overwritten).
4. The lambda layer has to follow a specific file structure for the lamba to recognize it:
    1. Create a folder called nodejs.
    2. Move the node_modules folder into the nodejs folder.
    - If you copy the folder, it will not copy the symbolic link to the entities import.
    - Alternatively, in the command line, symbolic links can be included in  a copy using 'cp -rL [source] [destination]'. 
        - The -L option stands for looking up and following symbolic links.
    3. Zip the nodejs folder.
        - Make sure to zip the nodejs folder itself, and not just the folder's contents.
5. Upload the zipped nodejs folder to s3. 
6. Set up the lambda layer in aws.
    1. Navigate to the lambda layer.
        1. Open up the aws lambda service.
        2. Lambda layer will be one of the options on the left panel.
    2. Click create layer and setup the layer.
        1. Set the compatible runtime to Node.js 18.x. Otherwise, the layer will not be accessible from the lambda.
        2. Set the code from the s3 bucket as the lambda layer's code.
7. Add the lambda layer to each lambda as a custom lambda layer.