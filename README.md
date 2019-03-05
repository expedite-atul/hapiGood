# hapiGood
# Registering good plugin of hapijs to log api responses
# Create a server.js file 
## do install your required dependecies !! [hapijs and other dependencies]
## for installing good plugin follow steps
    1-> npm i good --save
    2-> npm i good-squeeze --save
    3-> npm i good-file --save
    try with these three dependencies and if it requires console module if you want to log on console 
    4-> npm i good-console --save
          `use strict`;
          //--------------------------------connect to hapi server localhost:3000------------------------------------------//
          const Hapi = require('hapi');
          var Joi = require('joi');
          const server = Hapi.server({
              port: 8000,
              host: 'localhost',
          });
       //GOOD plugin options are here !!
          const options = {
              ops: {
                  interval: 1000
              },
              /* In case you want console all logs can follow this reporter type
              reporters: {
                  myConsoleReporter: [
                      {
                          module: 'good-squeeze',
                          name: 'Squeeze',
                          args: [{ log: '*', response: '*' }]
                      },
                      {
                          module: 'good-console'
                      },
                      'stdout'
                  ]
              }
              */
              /* Like if you're using console to log then all the logs will be lost because they're not getting stored       
              anywhere now if you want to store all obtained logs in a file follow this reporter this will create a log 
              folder and thus store all the logs under server_log file*/ 
              reporters: {
                file: 
                [{
                  module: 'good-squeeze',
                  name: 'Squeeze',
                  args: [{
                    log: '*',
                    response: '*'
                  }]
                }, {
                  module: 'good-squeeze',
                  name: 'SafeJson'
                }, {
                  module: 'good-file',
                  args: ['./Logs/server_log']
                }]
              }
            };
          const init = async () => {
           // registering good plugin here   
              await server.register({
                  plugin: require('good'),
                  options,
              });
              await server.start();
              console.info(`Server running at: ${server.info.uri}`);
          };
          process.on('unhandledRejection', (err) => {
              console.log(err);
              process.exit(1);
          });

          init();
          
          
          
          
          
