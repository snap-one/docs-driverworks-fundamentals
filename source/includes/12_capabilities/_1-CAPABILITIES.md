## Capabilities

Capabilities tell the Proxy what your device can do. Not all devices of a particular type have the same capabilities.  In these cases, the .c4z file contains information which enables the proxy driver to determine what capabilities are available.  For example, a Receiver-Tuner might have the included example code to the right in the .c4z file:

	<capabilities>
	    <has_discrete_volume_control>True</has_discrete_volume_control>
	    <has_up_down_volume_control>True</has_up_down_volume_control>
	    <has_discrete_input_select>True</has_discrete_input_select>
	    <has_toad_input_select>True</has_toad_input_select>
	    <has_discrete_surround_mode_select>True</has_discrete_surround_mode_select>
	    <has_toad_surround_mode_select>True</has_toad_surround_mode_select>
	    <has_tune_up_down>True</has_tune_up_down>
	    <has_search_up_down>False</has_search_up_down>
	    <has_discrete_preset>True</has_discrete_preset>
	    <has_preset_up_down>True</has_preset_up_down>
	    <preset_count>40</preset_count>
	    <can_upclass>False</can_upclass>
	    <can_downclass>False</can_downclass>
	    <video_provider_count>1</video_provider_count>
	    <video_consumer_count>4</video_consumer_count>
	    <audio_provider_count>2</audio_provider_count>
	    <audio_consumer_count>8</audio_consumer_count>
	    <has_discrete_mute_control>True</has_discrete_mute_control>
	    <has_toggle_mute_control>True</has_toggle_mute_control>
	    <has_discrete_channel_select>True</has_discrete_channel_select>
	    <has_discrete_input_select>True</has_discrete_input_select>
	    <has_channel_up_down>True</has_channel_up_down>
	    <has_feedback>True</has_feedback>
	    <serialdelay>0</serialdelay>
	    <serialsettings>9600 8 none 1 none 232</serialsettings>
	 </capabilities>

Please see the Proxy and Protocol Guide for supported capabilities for each of the device proxies.