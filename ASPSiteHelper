# coding: utf-8

"""
Author: Karakonstantioglou Spiros, "SaturnTheIcy" (kronos.ice.dev@gmail.com)

A simple helper class to help you with ASP sites.

"""

from urllib.parse import urlencode


class ASPSiteHelper():
	"""
	Help with asp site
		:param response: Scrapy response requier
	"""

	def __init__(self, response, base_url):
		self.response = response
		self.download_url = base_url

	def get_form_data(self, selectcssform, selectcssdopostback):
		"""
		Get url and data from form and doPostBack

		The return dict values are url and data.

		Data is a dictionary to add more values need by the site then

		call encode_post_data

			:rtype: dict
		"""
		result = dict()
		result['url'] = self.get_form_action(selectcssform)
		data = self.get_asp_form(selectcssform)
		data.update(self.get_do_post_back(selectcssdopostback))
		result['data'] = data
		return result

	# TODO : set dowload_url on asp helprt class
	def get_form_action(self, selectcss):
		""" Get from form action """
		if '::attr(action)' not in selectcss:
			selectcss += '::attr(action)'
		# select form url
		url = self.response.css(selectcss).extract_first(default=str())
		if url:
			if url.startswith('.'):
				url = url[1:]
			if url.startswith('/'):
				url = url[1:]
			if not url.startswith('http'):
				url = self.download_url + url
			return url
		return None

	def get_do_post_back(self, selectcss):
		""" Get javascript doPostBack from href url """
		if not selectcss.endswith('::attr(href)'):
			selectcss += '::attr(href)'

		regex_pattern = "'(.*?)'"
		data = self.response.css(selectcss).re(regex_pattern)
		result = False
		if data:
			result = dict()
			# select first parameter
			result["__EVENTTARGET"] = data[0]
			# select second parameter
			result["__EVENTARGUMENT"] = data[1]
		return result

	def get_asp_form(self, selectcss):
		""" Get asp form input data """
		if not selectcss:
			selectcss += 'form input'
		if not selectcss.endswith('input'):
			selectcss += ' input'

		form_data = dict()
		for item in self.response.css(selectcss):
			key = item.css('::attr(name)').extract_first(default='')
			value = item.css('::attr(value)').extract_first(default='')
			if key:
				form_data[key] = value
		return form_data

	def encode_post_data(self, post_data):
		""" Encode post data with urlencode """
		body = urlencode(post_data)
		return body
