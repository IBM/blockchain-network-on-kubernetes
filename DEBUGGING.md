Troubleshooting
===============

Jupyter Notebooks
-----------------

* Make sure the pip install ran correctly. You might need to restart the
  kernel and run the cells from the top after the pip install runs the first
  time.
* Many of the cells rely on variables that are set in earlier cells. Some of
  these are cleared in later cells. Start over at the top when troubleshooting.
* Many of the cells rely on service credentials from Bluemix that are set in
  earlier cells. Make sure to add your service credentials correctly.

Node-RED
--------

* Make sure that the websocket URL has been updated with the NODERED_BASE_URL in the HTML code. Refer to section `5. Update the websocket URL in HTML code` in the documentation for details.
