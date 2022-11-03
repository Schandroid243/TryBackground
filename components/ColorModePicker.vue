<template>
  <div class="bg-gray-400">
  <b-container>
    <b-row class="d-flex justify-content-center rowTitle">
      <h3>Create a User</h3>
    </b-row>
    <b-row align-h="center">
      <b-col cols="12" md="9">
        <b-form>
          <b-form-group>
            <b-input
              id="name"
              type="text"
              placeholder="Enter name"
              required>
            </b-input>
          </b-form-group>
          <b-form-group>
            <b-input
              id="username"
              type="text"
              placeholder="Enter username"
              required>
            </b-input>
          </b-form-group>
          <b-form-group>
            <b-input
              id="email"
              type="email"
              placeholder="Enter email"
              required>
            </b-input>
          </b-form-group>
          <b-form-group>
            <b-input
              id="phonenumber"
              type="tel"
              placeholder="Enter a phone number"
              required>
            </b-input>
          </b-form-group>
          <b-form-group>
            <b-input
              id="password"
              type="password"
              placeholder="Enter password"
              required>
            </b-input>
          </b-form-group>
          <b-row>
            <b-col cols="12" md="2">
              <b-button variant="primary" type="submit">Submit</b-button>
            </b-col>
            <b-col cols="12" md="1">
              <b-button type="reset">Reset</b-button>
            </b-col>
          </b-row>
        </b-form>
      </b-col>
    </b-row>
  </b-container>
  </div>
</template>
<style>
  .bg-gray-400 {
    background-color: darkgray
  }
</style>
