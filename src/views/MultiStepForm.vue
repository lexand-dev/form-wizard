<template>
  <div class="bg-white py-8 px-4">
    <div class="max-w-4xl md:max-w-7xl mx-auto md:flex md:space-x-16">
      <div class="md:w-2/4">
        <!-- Progress Bar -->
        <div class="mb-8">
          <div class="flex justify-between items-center mb-4">
            <span class="text-sm font-medium text-gray-900">Step {{ currentStep }} of 6</span>
            <span class="text-sm text-gray-600">{{ Math.round(progress) }}% Complete</span>
          </div>
          <div class="w-full bg-gray-200 rounded-full h-2">
            <div
              class="bg-blue-600 h-2 rounded-full transition-all duration-300"
              :style="`width: ${progress}%`"
            ></div>
          </div>
        </div>

        <!-- Step  Icons Indicators -->
        <div class="flex justify-between mb-8 overflow-x-auto pb-2">
          <div
            v-for="(step, index) in steps"
            :key="step.id"
            :class="[
              'flex flex-col items-center min-w-0 flex-1',
              { 'mr-4': index !== steps.length - 1 },
            ]"
          >
            <div
              :class="[
                'w-10 h-10 rounded-full flex items-center justify-center mb-2 transition-colors',
                {
                  'bg-blue-600 text-white': step.id < currentStep,
                  'bg-blue-100 text-blue-600': step.id === currentStep,
                  'bg-gray-200 text-gray-500': step.id > currentStep,
                },
              ]"
            >
              <component :is="step.icon" class="w-5 h-5" />
            </div>
            <div class="text-center">
              <p
                :class="[
                  'text-xs font-medium',
                  step.id === currentStep ? 'text-gray-900' : 'text-gray-500',
                ]"
              >
                {{ step.title }}
              </p>
              <p class="text-xs text-gray-500 hidden sm:block">{{ step.description }}</p>
            </div>
          </div>
        </div>

        <!-- Form Content -->
        <div class="bg-white border border-gray-200 rounded-lg shadow-sm mb-8">
          <div class="px-6 py-4 border-b border-gray-200">
            <h2 class="text-lg font-bold">{{ steps[currentStep - 1].title }}</h2>
            <p class="text-sm text-gray-600">{{ steps[currentStep - 1].description }}</p>
          </div>
          <div class="p-6">
            <Form ref="formRef" @submit="handleSubmit">
              <!-- Step 1: Personal Information -->
              <div v-if="currentStep === 1" class="space-y-4">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                  <div class="space-y-2">
                    <label for="firstName" class="text-sm font-medium text-gray-900"
                      >First Name *</label
                    >
                    <Field
                      name="firstName"
                      rules="required"
                      v-slot="{ field, errors }"
                      v-model="formData.firstName"
                    >
                      <input
                        v-bind="field"
                        id="firstName"
                        type="text"
                        :class="[
                          'flex h-10 w-full rounded-md border px-3 py-2 text-sm placeholder:text-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                          errors.length ? 'border-red-500' : 'border-gray-300',
                        ]"
                      />
                    </Field>
                    <ErrorMessage name="firstName" class="text-sm text-red-600" />
                  </div>
                  <div class="space-y-2">
                    <label for="lastName" class="text-sm font-medium text-gray-900"
                      >Last Name *</label
                    >
                    <Field
                      name="lastName"
                      rules="required"
                      v-slot="{ field, errors }"
                      v-model="formData.lastName"
                    >
                      <input
                        v-bind="field"
                        id="lastName"
                        type="text"
                        :class="[
                          'flex h-10 w-full rounded-md border px-3 py-2 text-sm placeholder:text-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                          errors.length ? 'border-red-500' : 'border-gray-300',
                        ]"
                      />
                    </Field>
                    <ErrorMessage name="lastName" class="text-sm text-red-600" />
                  </div>
                </div>
                <div class="space-y-2">
                  <label for="email" class="text-sm font-medium text-gray-900"
                    >Email Address *</label
                  >
                  <Field
                    name="email"
                    rules="required|email"
                    v-slot="{ field, errors }"
                    v-model="formData.email"
                  >
                    <input
                      v-bind="field"
                      id="email"
                      type="email"
                      :class="[
                        'flex h-10 w-full rounded-md border px-3 py-2 text-sm placeholder:text-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                        errors.length ? 'border-red-500' : 'border-gray-300',
                      ]"
                    />
                  </Field>
                  <ErrorMessage name="email" class="text-sm text-red-600" />
                </div>
                <div class="space-y-2">
                  <label for="phone" class="text-sm font-medium text-gray-900"
                    >Phone Number *</label
                  >
                  <Field
                    name="phone"
                    rules="required"
                    v-slot="{ field, errors }"
                    v-model="formData.phone"
                  >
                    <input
                      v-bind="field"
                      id="phone"
                      type="tel"
                      :class="[
                        'flex h-10 w-full rounded-md border px-3 py-2 text-sm placeholder:text-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                        errors.length ? 'border-red-500' : 'border-gray-300',
                      ]"
                    />
                  </Field>
                  <ErrorMessage name="phone" class="text-sm text-red-600" />
                </div>
              </div>

              <!-- Step 2: Company Information -->
              <div v-if="currentStep === 2" class="space-y-4">
                <div class="space-y-2">
                  <label for="companyName" class="text-sm font-medium text-gray-900"
                    >Company Name *</label
                  >
                  <Field
                    name="companyName"
                    rules="required"
                    v-slot="{ field, errors }"
                    v-model="formData.companyName"
                  >
                    <input
                      v-bind="field"
                      id="companyName"
                      type="text"
                      :class="[
                        'flex h-10 w-full rounded-md border px-3 py-2 text-sm placeholder:text-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                        errors.length ? 'border-red-500' : 'border-gray-300',
                      ]"
                    />
                  </Field>
                  <ErrorMessage name="companyName" class="text-sm text-red-600" />
                </div>
                <div class="space-y-2">
                  <label for="position" class="text-sm font-medium text-gray-900"
                    >Your Position *</label
                  >
                  <Field
                    name="position"
                    rules="required"
                    v-slot="{ field, errors }"
                    v-model="formData.position"
                  >
                    <input
                      v-bind="field"
                      id="position"
                      type="text"
                      :class="[
                        'flex h-10 w-full rounded-md border px-3 py-2 text-sm placeholder:text-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                        errors.length ? 'border-red-500' : 'border-gray-300',
                      ]"
                    />
                  </Field>
                  <ErrorMessage name="position" class="text-sm text-red-600" />
                </div>
                <div class="space-y-2">
                  <label for="industry" class="text-sm font-medium text-gray-900">Industry *</label>
                  <Field
                    name="industry"
                    rules="required"
                    v-slot="{ field, errors }"
                    v-model="formData.industry"
                  >
                    <select
                      v-bind="field"
                      id="industry"
                      :class="[
                        'flex h-10 w-full rounded-md border px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                        errors.length ? 'border-red-500' : 'border-gray-300',
                      ]"
                    >
                      <option value="">Select your industry</option>
                      <option value="technology">Technology</option>
                      <option value="healthcare">Healthcare</option>
                      <option value="finance">Finance</option>
                      <option value="education">Education</option>
                      <option value="retail">Retail</option>
                      <option value="manufacturing">Manufacturing</option>
                      <option value="other">Other</option>
                    </select>
                  </Field>
                  <ErrorMessage name="industry" class="text-sm text-red-600" />
                </div>
                <div class="space-y-2">
                  <label for="companySize" class="text-sm font-medium text-gray-900"
                    >Company Size *</label
                  >
                  <Field
                    name="companySize"
                    rules="required"
                    v-slot="{ field, errors }"
                    v-model="formData.companySize"
                  >
                    <select
                      v-bind="field"
                      id="companySize"
                      :class="[
                        'flex h-10 w-full rounded-md border px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                        errors.length ? 'border-red-500' : 'border-gray-300',
                      ]"
                    >
                      <option value="">Select company size</option>
                      <option value="1-10">1-10 employees</option>
                      <option value="11-50">11-50 employees</option>
                      <option value="51-200">51-200 employees</option>
                      <option value="201-1000">201-1000 employees</option>
                      <option value="1000+">1000+ employees</option>
                    </select>
                  </Field>
                  <ErrorMessage name="companySize" class="text-sm text-red-600" />
                </div>
              </div>

              <!-- Step 3: Project Details -->
              <div v-if="currentStep === 3" class="space-y-4">
                <div class="space-y-2">
                  <label for="projectType" class="text-sm font-medium text-gray-900"
                    >Project Type *</label
                  >
                  <Field
                    name="projectType"
                    rules="required"
                    v-slot="{ field, errors }"
                    v-model="formData.projectType"
                  >
                    <select
                      v-bind="field"
                      id="projectType"
                      :class="[
                        'flex h-10 w-full rounded-md border px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                        errors.length ? 'border-red-500' : 'border-gray-300',
                      ]"
                    >
                      <option value="">Select project type</option>
                      <option value="web-development">Web Development</option>
                      <option value="mobile-app">Mobile App</option>
                      <option value="consulting">Consulting</option>
                      <option value="design">Design</option>
                      <option value="integration">System Integration</option>
                      <option value="other">Other</option>
                    </select>
                  </Field>
                  <ErrorMessage name="projectType" class="text-sm text-red-600" />
                </div>
                <div class="space-y-2">
                  <label for="projectDescription" class="text-sm font-medium text-gray-900"
                    >Project Description *</label
                  >
                  <Field
                    name="projectDescription"
                    rules="required"
                    v-slot="{ field, errors }"
                    v-model="formData.projectDescription"
                  >
                    <textarea
                      v-bind="field"
                      id="projectDescription"
                      rows="4"
                      placeholder="Describe your project in detail..."
                      :class="[
                        'flex w-full rounded-md border px-3 py-2 text-sm placeholder:text-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                        errors.length ? 'border-red-500' : 'border-gray-300',
                      ]"
                    ></textarea>
                  </Field>
                  <ErrorMessage name="projectDescription" class="text-sm text-red-600" />
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                  <div class="space-y-2">
                    <label for="timeline" class="text-sm font-medium text-gray-900"
                      >Timeline *</label
                    >
                    <Field
                      name="timeline"
                      rules="required"
                      v-slot="{ field, errors }"
                      v-model="formData.timeline"
                    >
                      <select
                        v-bind="field"
                        id="timeline"
                        :class="[
                          'flex h-10 w-full rounded-md border px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                          errors.length ? 'border-red-500' : 'border-gray-300',
                        ]"
                      >
                        <option value="">Select timeline</option>
                        <option value="asap">ASAP</option>
                        <option value="1-month">Within 1 month</option>
                        <option value="3-months">Within 3 months</option>
                        <option value="6-months">Within 6 months</option>
                        <option value="flexible">Flexible</option>
                      </select>
                    </Field>
                    <ErrorMessage name="timeline" class="text-sm text-red-600" />
                  </div>
                  <div class="space-y-2">
                    <label for="budget" class="text-sm font-medium text-gray-900"
                      >Budget Range *</label
                    >
                    <Field
                      name="budget"
                      rules="required"
                      v-slot="{ field, errors }"
                      v-model="formData.budget"
                    >
                      <select
                        v-bind="field"
                        id="budget"
                        :class="[
                          'flex h-10 w-full rounded-md border px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent',
                          errors.length ? 'border-red-500' : 'border-gray-300',
                        ]"
                      >
                        <option value="">Select budget range</option>
                        <option value="under-5k">Under $5,000</option>
                        <option value="5k-15k">$5,000 - $15,000</option>
                        <option value="15k-50k">$15,000 - $50,000</option>
                        <option value="50k-100k">$50,000 - $100,000</option>
                        <option value="over-100k">Over $100,000</option>
                      </select>
                    </Field>
                    <ErrorMessage name="budget" class="text-sm text-red-600" />
                  </div>
                </div>
              </div>

              <!-- Step 4: Requirements -->
              <div v-if="currentStep === 4" class="space-y-4">
                <div class="space-y-3">
                  <label class="text-sm font-medium text-gray-900">Services Required *</label>
                  <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                    <div
                      v-for="service in serviceOptions"
                      :key="service"
                      class="flex items-center space-x-2"
                    >
                      <input
                        :id="service"
                        type="checkbox"
                        :value="service"
                        v-model="formData.services"
                        class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300 rounded"
                      />
                      <label :for="service" class="text-sm font-normal text-gray-900">
                        {{ service }}
                      </label>
                    </div>
                  </div>
                  <p v-if="servicesError" class="text-sm text-red-600">{{ servicesError }}</p>
                </div>
                <div class="space-y-3">
                  <label class="text-sm font-medium text-gray-900">Project Priority *</label>
                  <Field name="priority" rules="required" v-model="formData.priority">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                      <div class="flex items-center space-x-2">
                        <input
                          id="low"
                          type="radio"
                          value="low"
                          v-model="formData.priority"
                          class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300"
                        />
                        <label for="low" class="text-sm font-medium text-gray-900"
                          >Low Priority</label
                        >
                      </div>
                      <div class="flex items-center space-x-2">
                        <input
                          id="medium"
                          type="radio"
                          value="medium"
                          v-model="formData.priority"
                          class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300"
                        />
                        <label for="medium" class="text-sm font-medium text-gray-900"
                          >Medium Priority</label
                        >
                      </div>
                      <div class="flex items-center space-x-2">
                        <input
                          id="high"
                          type="radio"
                          value="high"
                          v-model="formData.priority"
                          class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300"
                        />
                        <label for="high" class="text-sm font-medium text-gray-900"
                          >High Priority</label
                        >
                      </div>
                    </div>
                  </Field>
                  <ErrorMessage name="priority" class="text-sm text-red-600" />
                </div>
                <div class="space-y-2">
                  <label for="additionalNotes" class="text-sm font-medium text-gray-900"
                    >Additional Notes</label
                  >
                  <textarea
                    id="additionalNotes"
                    v-model="formData.additionalNotes"
                    rows="3"
                    placeholder="Any additional requirements or notes..."
                    class="flex w-full rounded-md border border-gray-300 px-3 py-2 text-sm placeholder:text-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  ></textarea>
                </div>
              </div>

              <!-- Step 5: Preferences -->
              <div v-if="currentStep === 5" class="space-y-4">
                <div class="space-y-3">
                  <label class="text-sm font-medium text-gray-900"
                    >Preferred Communication Method *</label
                  >
                  <Field
                    name="communicationMethod"
                    rules="required"
                    v-model="formData.communicationMethod"
                  >
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                      <div class="flex items-center space-x-2">
                        <input
                          id="email-comm"
                          type="radio"
                          value="email"
                          v-model="formData.communicationMethod"
                          class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300"
                        />
                        <label for="email-comm" class="text-sm font-medium text-gray-900"
                          >Email</label
                        >
                      </div>
                      <div class="flex items-center space-x-2">
                        <input
                          id="phone-comm"
                          type="radio"
                          value="phone"
                          v-model="formData.communicationMethod"
                          class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300"
                        />
                        <label for="phone-comm" class="text-sm font-medium text-gray-900"
                          >Phone</label
                        >
                      </div>
                      <div class="flex items-center space-x-2">
                        <input
                          id="video-comm"
                          type="radio"
                          value="video"
                          v-model="formData.communicationMethod"
                          class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300"
                        />
                        <label for="video-comm" class="text-sm font-medium text-gray-900"
                          >Video Call</label
                        >
                      </div>
                    </div>
                  </Field>
                  <ErrorMessage name="communicationMethod" class="text-sm text-red-600" />
                </div>
                <div class="space-y-3">
                  <label class="text-sm font-medium text-gray-900">Meeting Preference *</label>
                  <Field
                    name="meetingPreference"
                    rules="required"
                    v-model="formData.meetingPreference"
                  >
                    <div class="space-y-2">
                      <div class="flex items-center space-x-2">
                        <input
                          id="morning"
                          type="radio"
                          value="morning"
                          v-model="formData.meetingPreference"
                          class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300"
                        />
                        <label for="morning" class="text-sm font-medium text-gray-900"
                          >Morning (9 AM - 12 PM)</label
                        >
                      </div>
                      <div class="flex items-center space-x-2">
                        <input
                          id="afternoon"
                          type="radio"
                          value="afternoon"
                          v-model="formData.meetingPreference"
                          class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300"
                        />
                        <label for="afternoon" class="text-sm font-medium text-gray-900"
                          >Afternoon (12 PM - 5 PM)</label
                        >
                      </div>
                      <div class="flex items-center space-x-2">
                        <input
                          id="evening"
                          type="radio"
                          value="evening"
                          v-model="formData.meetingPreference"
                          class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300"
                        />
                        <label for="evening" class="text-sm font-medium text-gray-900"
                          >Evening (5 PM - 8 PM)</label
                        >
                      </div>
                      <div class="flex items-center space-x-2">
                        <input
                          id="flexible-time"
                          type="radio"
                          value="flexible"
                          v-model="formData.meetingPreference"
                          class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300"
                        />
                        <label for="flexible-time" class="text-sm font-medium text-gray-900"
                          >Flexible</label
                        >
                      </div>
                    </div>
                  </Field>
                  <ErrorMessage name="meetingPreference" class="text-sm text-red-600" />
                </div>
                <div class="flex items-center space-x-2">
                  <input
                    id="newsletter"
                    type="checkbox"
                    v-model="formData.newsletter"
                    class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300 rounded"
                  />
                  <label for="newsletter" class="text-sm text-gray-900">
                    Subscribe to our newsletter for updates and insights
                  </label>
                </div>
              </div>

              <!-- Step 6: Review -->
              <div v-if="currentStep === 6" class="space-y-6">
                <!--   <div class="space-y-4">
                  <h3 class="text-lg font-bold text-gray-900">Review Your Information</h3>

                  <div class="grid gap-4">
                    <div class="p-4 bg-gray-50 rounded-lg">
                      <h4 class="font-semibold mb-2 text-gray-900">Personal Information</h4>
                      <p class="text-sm text-gray-700">
                        <strong>Name:</strong> {{ formData.firstName }} {{ formData.lastName }}
                      </p>
                      <p class="text-sm text-gray-700">
                        <strong>Email:</strong> {{ formData.email }}
                      </p>
                      <p class="text-sm text-gray-700">
                        <strong>Phone:</strong> {{ formData.phone }}
                      </p>
                    </div>

                    <div class="p-4 bg-gray-50 rounded-lg">
                      <h4 class="font-semibold mb-2 text-gray-900">Company Information</h4>
                      <p class="text-sm text-gray-700">
                        <strong>Company:</strong> {{ formData.companyName }}
                      </p>
                      <p class="text-sm text-gray-700">
                        <strong>Position:</strong> {{ formData.position }}
                      </p>
                      <p class="text-sm text-gray-700">
                        <strong>Industry:</strong> {{ formData.industry }}
                      </p>
                      <p class="text-sm text-gray-700">
                        <strong>Size:</strong> {{ formData.companySize }}
                      </p>
                    </div>

                    <div class="p-4 bg-gray-50 rounded-lg">
                      <h4 class="font-semibold mb-2 text-gray-900">Project Details</h4>
                      <p class="text-sm text-gray-700">
                        <strong>Type:</strong> {{ formData.projectType }}
                      </p>
                      <p class="text-sm text-gray-700">
                        <strong>Timeline:</strong> {{ formData.timeline }}
                      </p>
                      <p class="text-sm text-gray-700">
                        <strong>Budget:</strong> {{ formData.budget }}
                      </p>
                      <p class="text-sm text-gray-700">
                        <strong>Description:</strong> {{ formData.projectDescription }}
                      </p>
                    </div>

                    <div class="p-4 bg-gray-50 rounded-lg">
                      <h4 class="font-semibold mb-2 text-gray-900">Requirements</h4>
                      <p class="text-sm text-gray-700">
                        <strong>Services:</strong> {{ formData.services.join(', ') }}
                      </p>
                      <p class="text-sm text-gray-700">
                        <strong>Priority:</strong> {{ formData.priority }}
                      </p>
                      <p v-if="formData.additionalNotes" class="text-sm text-gray-700">
                        <strong>Notes:</strong> {{ formData.additionalNotes }}
                      </p>
                    </div>

                    <div class="p-4 bg-gray-50 rounded-lg">
                      <h4 class="font-semibold mb-2 text-gray-900">Preferences</h4>
                      <p class="text-sm text-gray-700">
                        <strong>Communication:</strong> {{ formData.communicationMethod }}
                      </p>
                      <p class="text-sm text-gray-700">
                        <strong>Meeting Time:</strong> {{ formData.meetingPreference }}
                      </p>
                      <p class="text-sm text-gray-700">
                        <strong>Newsletter:</strong> {{ formData.newsletter ? 'Yes' : 'No' }}
                      </p>
                    </div>
                  </div>
                </div> -->

                <div class="flex items-center space-x-2">
                  <Field name="termsAccepted" rules="required" v-slot="{ field, errors }">
                    <input
                      v-bind="field"
                      v-model="formData.termsAccepted"
                      id="terms"
                      type="checkbox"
                      :class="[
                        'h-4 w-4 text-blue-600 focus:ring-blue-500 rounded',
                        errors.length ? 'border-red-500' : 'border-gray-300',
                      ]"
                    />
                  </Field>
                  <label for="terms" class="text-sm text-gray-900">
                    I accept the terms and conditions and privacy policy *
                  </label>
                </div>
                <ErrorMessage name="termsAccepted" class="text-sm text-red-600" />
              </div>
            </Form>
          </div>
        </div>

        <!-- Navigation Buttons -->
        <div class="flex justify-between">
          <button
            @click="prevStep"
            :disabled="currentStep === 1"
            class="min-w-24 px-4 py-2 text-sm font-medium text-gray-700 bg-white border border-gray-300 rounded-md hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 disabled:opacity-50 disabled:cursor-not-allowed"
          >
            Previous
          </button>

          <div class="flex gap-2">
            <button
              @click="saveForLater"
              class="px-4 py-2 text-sm font-medium text-gray-600 bg-transparent hover:text-gray-800 focus:outline-none"
            >
              Save for Later
            </button>

            <button
              v-if="currentStep === 6"
              @click="submitForm"
              class="min-w-24 px-4 py-2 text-sm font-medium text-white bg-blue-600 border border-transparent rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
            >
              Submit
            </button>
            <button
              v-else
              @click="nextStep"
              class="min-w-24 px-4 py-2 text-sm font-medium text-white bg-blue-600 border border-transparent rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
            >
              Next
            </button>
          </div>
        </div>
      </div>

      <!-- Document Preview -->
      <div class="flex-1 hidden md:block order-2">
        <h2 class="text-xl font-bold mb-4">Vista Previa del Contrato</h2>
        <div class="border p-4 bg-white whitespace-pre-wrap shadow-md rounded-md">
          {{ documentPreview }}
        </div>
        <button
          :disabled="currentStep !== 6"
          class="mt-4 bg-green-500 text-white px-4 py-2 rounded disabled:bg-gray-300 disabled:text-gray-500 disabled:cursor-not-allowed"
          @click="descargarWord"
        >
          Descargar Word
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import { Form, Field, ErrorMessage } from 'vee-validate'
import { defineRule } from 'vee-validate'
import { saveAs } from 'file-saver'
import { Document, Packer, Paragraph, TextRun } from 'docx'
import {
  UserIcon,
  BuildingIcon,
  FileTextIcon,
  SettingsIcon,
  CreditCardIcon,
  SendIcon,
} from 'lucide-vue-next'
import { required, email } from '@vee-validate/rules'

// Define validation rules
defineRule('required', required)
defineRule('email', email)

// Reactive data
const currentStep = ref(1)
const formRef = ref(null)

const formData = ref({
  firstName: '',
  lastName: '',
  email: '',
  phone: '',
  companyName: '',
  position: '',
  industry: '',
  companySize: '',
  projectType: '',
  projectDescription: '',
  timeline: '',
  budget: '',
  services: [],
  priority: '',
  additionalNotes: '',
  communicationMethod: '',
  meetingPreference: '',
  newsletter: false,
  termsAccepted: false,
})

const servicesError = ref('')
console.log('Form data initialized:', formData.value.termsAccepted)

const serviceOptions = [
  'Strategy & Planning',
  'UI/UX Design',
  'Frontend Development',
  'Backend Development',
  'Database Design',
  'API Integration',
  'Testing & QA',
  'Deployment & Hosting',
]

const steps = [
  { id: 1, title: 'Personal Info', icon: UserIcon, description: 'Basic personal details' },
  { id: 2, title: 'Company', icon: BuildingIcon, description: 'Company information' },
  { id: 3, title: 'Project', icon: FileTextIcon, description: 'Project details' },
  { id: 4, title: 'Requirements', icon: SettingsIcon, description: 'Service requirements' },
  { id: 5, title: 'Preferences', icon: CreditCardIcon, description: 'Communication preferences' },
  { id: 6, title: 'Review', icon: SendIcon, description: 'Review and submit' },
]

// Computed properties
const progress = computed(() => (currentStep.value / 6) * 100)

// Watch for services validation
watch(
  () => formData.value.services,
  (newServices) => {
    if (newServices.length > 0) {
      servicesError.value = ''
    }
  },
)

// Methods
const validateCurrentStep = async () => {
  if (currentStep.value === 4) {
    // Custom validation for services
    if (formData.value.services.length === 0) {
      servicesError.value = 'At least one service is required'
      return false
    }
  }

  // Use VeeValidate's validation
  const { errors } = await formRef.value.validate()
  return Object.keys(errors).length === 0
}

const nextStep = async () => {
  const isValid = await validateCurrentStep()
  if (isValid && currentStep.value < 6) {
    currentStep.value++
  }
}

const prevStep = () => {
  if (currentStep.value > 1) {
    currentStep.value--
  }
}

const saveForLater = () => {
  console.log('Saving form data for later:', formData.value)
  alert('Form data saved!')
}

const submitForm = async () => {
  const isValid = await validateCurrentStep()
  if (isValid) {
    console.log('Form submitted:', formData.value)
    alert('Form submitted successfully!')
  }
}

const handleSubmit = (values) => {
  console.log('Form values:', values)
}

// Document preview
const documentPreview = computed(() => {
  const d = formData.value
  return `
PROJECT SUMMARY

Personal Information
Name: ${[d.firstName, d.lastName].filter(Boolean).join(' ') || '—'}
Email: ${d.email || '—'}
Phone: ${d.phone || '—'}

Company Information
Company: ${d.companyName || '—'}
Position: ${d.position || '—'}
Industry: ${d.industry || '—'}
Size: ${d.companySize || '—'}

Project Details
Type: ${d.projectType || '—'}
Timeline: ${d.timeline || '—'}
Budget: ${d.budget || '—'}
Description:
${d.projectDescription || '—'}

Requirements
Services: ${d.services?.length ? d.services.join(', ') : '—'}
Priority: ${d.priority || '—'}
Notes:
${d.additionalNotes || '—'}

Preferences
Communication: ${d.communicationMethod || '—'}
Meeting Time: ${d.meetingPreference || '—'}
Newsletter: ${d.newsletter ? 'Yes' : 'No'}

Confirmation
Terms Accepted: ${d.termsAccepted ? 'Yes' : 'No'}
`.trim()
})

const descargarWord = () => {
  const doc = new Document({
    sections: [
      {
        children: [
          new Paragraph({
            children: [new TextRun(documentPreview.value)],
          }),
        ],
      },
    ],
  })

  Packer.toBlob(doc).then((blob) => {
    saveAs(blob, 'contrato.docx')
  })
}
</script>
