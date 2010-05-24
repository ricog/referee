# Referee Plugin for CakePHP 1.3+

Referee plugin catches errors and dispatches them to custom listeners.

> "*If you're waiting around for users to tell you about problems with your website or application, you're only seeing a tiny fraction of all the problems that are actually occurring. The proverbial tip of the iceberg.*

> *Also, if this is the case, I'm sorry to be the one to have to tell you this, but you kind of suck at your job -- **which is to know more about your application's health than your users do**.*" - [Exception Driven Development][1]

## Features
 * Easily extended
 * Logs all (*even fatal*) errors and exceptions

## Installation

* Download the plugin

        $ cd /path/to/your/app/plugins && git clone git://github.com/joebeeson/referee.git

* Create the schema

        $ cake schema create Referee.schema

* Add the component and attach your listeners

        public $components = array(
            'Referee.Whistle' => array(
                'listeners' => array(
                    'DbLog',
                    'SysLog'
                )
            )
        );

## Listeners

Listeners perform the actual leg work in handling an error. Their only requirement is that they have a public function that the `WhistleComponent` can trigger when it needs to notify the listener of an error. 

You can configure listeners in the `$components` declaration for the `WhistleComponent`, here's an example of some configuration options...

        public $components = array(
            'Referee.Whistle' => array(
                'listeners' => array(
 
                    'YourLogger' => array(

                        // The error type(s) that should trigger this listener
                        'levels' => E_ERROR,

                        // The method to call to pass an error, defaults to 'error'
                        'method' => 'customMethod',

                        // The class to instantiate, defaults to (name)Listener, YourLoggerListener in this case
                        'class'  => 'yourCustomListenerClass',

                        'parameters' => array(
                            /**
                             * Anything in here will be passed to the listener's
                             * error method when an error occurs.
                             */
                        )

                    )

                )
            )
        );

If you prefer a more dynamic approach or just hate to see your `$components` array so large, you can use the `attachListener` method which accepts two parameters, the listener's name and an optional configuration for it -- the configuration allows the exact declarations in the examples above. The method will return a boolean to indicate success in attaching the listener.

The method that is invoked by the `WhistleComponent` should accept two parameters: `$error` and `$parameters` -- the `$error` is an associative array describing the error that occurred and `$parameters` is an array containing the parameters (*if any*) that were declared in the `$components` declaration for the listener.

  [1]: http://www.codinghorror.com/blog/2009/04/exception-driven-development.html
