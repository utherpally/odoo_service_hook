Usage
=======

Example add a new method in ``web.utils`` service

.. code-block:: 

	// File: extra_utils.js
	odoo.post_define('web.utils', function(utils, require) {
		return _.extend(utils,{
			pow: function(x) {
				return x * x;
			}
		})
	});

	/**
   * Add priority to change run order when hook same service.
   * (default priority: 13)
   *
	**/
	odoo.post_define('web.utils', 15, function(utils, require) {

		var awesome = require('awesome_other_service'); // Add another service as dependency

		return _.extend(utils,{
			// Override
			pow: function(x) {
				return awesome.pow(x);
			},
			// Add new
			sin: function(x) {
				return awesome.sin(x);
			}
		})

	});

	/* File: views/assets.xml */

	<template id="hook_assets_common" inherit_id="odoo_service_hook.hook_assets_common">
		<xpath expr="." position="inside">
			<script type="text/javascript" src="/your_module/static/src/js/extra_utils.js"></script>
		</xpath>
	</template>

	/* Also support: hook_assets_backend, hook_assets_frontend */


It will apply automatically:

.. code-block:: 

  odoo.define('using.web.utils', function(require){
  
      var utils = require('web.utils');
    // No extra require to ensure your hook run completed.
  
    var x = utils.pow(5);
    var y = utils.sin(1.5);
  
  })


